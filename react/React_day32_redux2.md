## やったこと
先日に引き続き、Reduxのdocを読む

### Redux Thunkについて
Redux-Thunkとは、ReduxのAction Creatorに非同期処理を実装するためのミドルウェア  
通常Action CreatorはActionオブジェクトを返しますが、Redux-Thunkを使用すると「`Thunk`」という関数を返すことができるようになる  
これによってActionのディスパッチを遅らせたり、特定の条件が満たされた場合のみディスパッチできるようになる  

次にこれを頑張って読んだ  
[Thunks in Redux: The Basics](https://medium.com/fullstack-academy/thunks-in-redux-the-basics-85e538a3fe60)  

仮にuserを作るdispatchを行いたいとする  
**Problem**  
```ts
// in an action creator module:
const asyncLogin = () =>
  axios.get('/api/auth/me')
  .then(res => res.data)
  .then(user => {
    // how do we use this user object?
  })

// somewhere in component:
store.dispatch(asyncLogin()) // nope; `asyncLogin()` is a promise, not action
```

このasyncLoginはactionではなくPromiseなので、dispatchでは扱うことができない  
一つの解決策として直接asyncを呼ぶ方法がある  

```ts

// in an action creator module:
import store from '../store'

const simpleLogin = user => ({ type: LOGIN, user })

const asyncLogin = () =>
  axios.get('/api/auth/me')
  .then(res => res.data)
  .then(user => {
    store.dispatch(simpleLogin(user))
  })

// somewhere in component:
asyncLogin()
```

しかしいくつか問題がある
1. 一貫性のないAPIになっている
> Now our components sometimes call store.dispatch(syncActionCreator()), and sometimes call doSomeAsyncThing().
2. asyncLoginが純粋な関数になっていない
3. スコープ内の特定のsotreに密接に結合されている
  再利用性がない

そこでthunkを使う方法がbetterである  
```ts
// in an action creator module:
import store from '../store'   // still coupled (for now...)

const simpleLogin = user => ({ type: LOGIN, user })

const thunkedLogin = () =>     // action creator, when invoked…
  () =>                        // …returns a thunk, which when invoked…
    axios.get('/api/auth/me')  // …performs the actual effect.
    .then(res => res.data)
    .then(user => {
      store.dispatch(simpleLogin(user))
    })

// somewhere in component:
store.dispatch(thunkedLogin()) // dispatches the thunk to the store.

// The thunk itself (`() => axios.get…`) has not yet been called.
```

これなら以前の純粋なAPIに戻ることができた。  
しかし、「そのactionの作成者は関数を返し、その関数はその後でディスパッチされる (最後の行)。  
Reduxはアクションオブジェクトしか理解できないのでは？それに、これはまだ密結合だ！」という指摘があるかもしれない  

そのため、さらに変更が必要になる  

#### Redux-Thunk Middlewareのinstall
redux-thunkをinstallするとdispatchが以下のように書き換えられる
```ts
actionOrThunk =>
  typeof actionOrThunk === 'function'
    ? actionOrThunk(dispatch, getState)
    : passAlong(actionOrThunk);
```
通常のactionオブジェクトが呼ばれた場合は通常のようにそれをreducerに渡す  
仮に関数がdispatchされた場合はredux-thunkはその関数を呼びstoreのdispatchとgetStateを渡す  
redux-thunkはreducerにthunkを転送しない  

これで統一されたAPIを使うことができ、純粋な関数を保つことができるようになった  

##### なぜPromiseではなくThunkなのか
Promiseを使うと、actionが不純になり再利用性が低下する  
もちろんPromiseを使うこともできるがThunkの方がよりシンプルなアプローチができる  

参考資料
- [redux-thunk](https://github.com/reduxjs/redux-thunk)  
- [Thunks in Redux: The Basics](https://medium.com/fullstack-academy/thunks-in-redux-the-basics-85e538a3fe60)

上の英語記事をわかりやすく解説してくれてるQiitaがあった  
[redux-thunk入門、簡単まとめ](https://qiita.com/hiroya8649/items/c202742c99d2cc6159b8)  

### Thunkについて
そのまま関数Aを利用するのではなく、まず関数Bに変数を提供して、関数Bはそれを使って関数Aの中身を完成させる。  
最後は完成した関数Aを返す、必要な時に関数Aを呼び出す流れ  
```js
function yell (text) {
  console.log(text + '!')
}

yell('bonjour') // 'bonjour!'

function thunkedYell (text) {
  return function thunk () {
    console.log(text + '!')
  }
}

const thunk = thunkedYell('bonjour') // まだ実行されてない

thunk() // 'bonjour!' //必要時に呼ぶ
```
複雑すぎてわからん...  

React / Redux はどういう風にThunkの仕組みを利用しているのかというと、  
主にはactions、 action creators、 componentsが「直接的に」データに(Asyncなどによる)影響を起こさせないようにしている  
それらの処理は全部Thunkに包んで、そのあとmiddlewareがThunk呼ぶ時に実行される  
このようの仕組みだと、少なくともMiddlewareレベル以外のところは比較的にピュア（Async関連の処理をしない）になる  

#### 純粋関数とは
ずっとpureの話が出てきたが何かわかっていなかった  
純粋関数の特徴は以下である。

- 引数が同じ場合、常に同じ返り値となる。（参照透過性）
- 副作用が発生しない

## createAsyncThunkについて
[Redux Toolkit で Async Thunk が曲者なので詳しく解説する](https://times.hrbrain.co.jp/entry/2020/12/08/redux-toolkit-async-thunk)  

createAsyncThunkの型  
```ts
function createAsyncThunk<
    Returned,
    ThunkArg = void,
    ThunkApiConfig extends AsyncThunkConfig = {}
>(
    typePrefix: string,
    payloadCreator: AsyncThunkPayloadCreator<Returned, ThunkArg, ThunkApiConfig>,
    options?: AsyncThunkOptions<ThunkArg, ThunkApiCongi>
): AsyncThunk<Returned, ThunkArg, ThunkApiConfig>;
```
```ts
function createAsyncThunk<
    第2引数の関数の返り値,
    第2引数の関数の第1引数の型（生成された関数を実行する時に必要な引数）,
    Thunkが引き回しているコンテキストの型
>(/* ... */)
```
#### 1. Returned
Returned は AsyncThunkPayloadCreator に渡されている  
```ts
type AsyncThunkPayloadCreator<
    Returned,
    ThunkArg = void,
    ThunkApiConfig extends AsyncThunkConfig = {}
> = (arg: ThunkArg, thunkAPI: GetThunkAPI<ThunkApiConfig>) => AsyncThunkPayloadCreatorReturnValue<Returned, ThunkApiConfig>;
```
`AsyncThunkPayloadCreator` から渡ってきた Returned は更に `AsyncThunkPayloadCreatorReturnValue` に渡されている  

```ts
type AsyncThunkPayloadCreatorReturnValue<
    Returned,
    ThunkApiConfig extends AsyncThunkConfig
> = Promise<Returned | RejectWithValue<GetRejectValue<ThunkApiConfig>>> | Returned | RejectWithValue<GetRejectValue<ThunkApiConfig>>;
```
入ってきた Returned がそのまま `Promise<T>` の中に渡されている  


### createAsyncThunkの使い方
[createAsyncThunk doc](https://redux-toolkit.js.org/api/createAsyncThunk)  
#### 概要
Reduxのアクションタイプ文字列とプロミスを返すべきコールバック関数を受け取る関数  
渡されたアクションタイプの文字列に基づいてPromiseのライフサイクルアクションタイプを生成し、Promiseのコールバックを実行し、  
返されたプロミスに基づいてライフサイクルアクションをdispatchするthunkActionCreaterを返す  

sample usage
```ts
import { createAsyncThunk, createSlice } from '@reduxjs/toolkit'
import { userAPI } from './userAPI'

// First, create the thunk
const fetchUserById = createAsyncThunk(
  'users/fetchByIdStatus',
  async (userId, thunkAPI) => {
    const response = await userAPI.fetchById(userId)
    return response.data
  }
)

// Then, handle actions in your reducers:
const usersSlice = createSlice({
  name: 'users',
  initialState: { entities: [], loading: 'idle' },
  reducers: {
    // standard reducer logic, with auto-generated action types per reducer
  },
  extraReducers: (builder) => {
    // Add reducers for additional action types here, and handle loading state as needed
    builder.addCase(fetchUserById.fulfilled, (state, action) => {
      // Add user to the state array
      state.entities.push(action.payload)
    })
  },
})

// Later, dispatch the thunk as needed in the app
dispatch(fetchUserById(123))
```

#### createAsyncThunkの受け取るパラメータ  
`type`, `payloadCreater`, `options`の三つ  

**`type`**   
例えば、`users/requestStatus`という文字列を渡した場合、  
- pending: `users/requestStatus/pending`
- fulfilled: `users/requestStatus/fulfilled`
- rejected: `users/requestStatus/rejected`
とそれぞれのaction typeが作成される  

**`payloadCreater`**  
callback関数は非同期を含むPromiseを返す必要がある  
同期的な値を返すこともある  
payloadCreaterは二つの引数を受け取る  

- `arg`
  - 必要な引数を受け取る  
  - 引数は1つしか使えないので、複数必要な場合はこのようにオブジェクトにする必要がある
  ```ts
  async (arg, thunkAPI) => {
    // 引数は1つしか使えないので、複数必要な場合はこのようにオブジェクトにする必要がある
    const { org, repo, issueId } = arg;
  ```
- `thunkAPI`
  - thunkAPIは以下のような値を持つ
  ```ts
  interface ThunkAPI {
    dispatch: Function;
    getState: Function;
    extra?: any;
    requestId: string;
    signal: AbortSignal;
  }
  ```

#### Return
返り値はdefaultのthunk action createrを返す  
thunk action createrには`pending`, `fulfilled`, `rejected`がネスとされたフィールドとして追加されている  

1. dispatchを実行すると、まずはpendingのactionを実行する  
2. payloadCreatorを呼んで、Promiseの結果を待つ
3. Promiseが正常に解決したら、Promiseの値をaction.payloadとしてfulfilledアクションをdispatchする
4. PromiseがrejectWithValueとして解決した場合、rejectedアクションをaction.payloadに渡された値とaction.error.messageとしてdispatchする
5. Promiseが失敗した場合、拒否されたアクションをシリアル化されたエラー値と共にaction.errorとしてdispatchする

### Promise Lifecycle Actions
createAsyncThunkは`createAction`を使用してredux actionを作成する  
それぞれの型は以下のようになっている  
```ts
interface SerializedError {
  name?: string
  message?: string
  code?: string
  stack?: string
}

interface PendingAction<ThunkArg> {
  type: string
  payload: undefined
  meta: {
    requestId: string
    arg: ThunkArg
  }
}

interface FulfilledAction<ThunkArg, PromiseResult> {
  type: string
  payload: PromiseResult
  meta: {
    requestId: string
    arg: ThunkArg
  }
}

interface RejectedAction<ThunkArg> {
  type: string
  payload: undefined
  error: SerializedError | any
  meta: {
    requestId: string
    arg: ThunkArg
    aborted: boolean
    condition: boolean
  }
}

interface RejectedWithValueAction<ThunkArg, RejectedValue> {
  type: string
  payload: RejectedValue
  error: { message: 'Rejected' }
  meta: {
    requestId: string
    arg: ThunkArg
    aborted: boolean
  }
}

type Pending = <ThunkArg>(
  requestId: string,
  arg: ThunkArg
) => PendingAction<ThunkArg>

type Fulfilled = <ThunkArg, PromiseResult>(
  payload: PromiseResult,
  requestId: string,
  arg: ThunkArg
) => FulfilledAction<ThunkArg, PromiseResult>

type Rejected = <ThunkArg>(
  requestId: string,
  arg: ThunkArg
) => RejectedAction<ThunkArg>

type RejectedWithValue = <ThunkArg, RejectedValue>(
  requestId: string,
  arg: ThunkArg
) => RejectedWithValueAction<ThunkArg, RejectedValue>
```

### Handling Thunk Results
```ts
// in the component
import { unwrapResult } from '@reduxjs/toolkit'

// in the component
const onClick = () => {
  dispatch(fetchUserById(userId))
    .then(unwrapResult)
    .then((originalPromiseResult) => {
      // handle result here
    })
    .catch((rejectedValueOrSerializedError) => {
      // handle result here
    })
}
```
unwrapResultを使うと成功時（fulfilled時）に.then(...)、失敗時（rejected時）に.catch(...)へと処理を分けてくれる  



