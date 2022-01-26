## やったこと
Redux Toolkitについて

## Quick Start
Install Redux Toolkit and React-Redux
```
$ npm install @reduxjs/toolkit react-redux
```
### 1. Create a Redux Store

```tsx
import { configureStore } from '@reduxjs/toolkit'

export const store = configureStore({
  reducer: {},
})

// Infer the `RootState` and `AppDispatch` types from the store itself
export type RootState = ReturnType<typeof store.getState>
// Inferred type: {posts: PostsState, comments: CommentsState, users: UsersState}
export type AppDispatch = typeof store.dispatch
```

#### configureSotre [doc(configureStore)](https://redux-toolkit.js.org/api/configureStore)
configureStore accepts a single configuration object parameter, with the following options:
```ts
interface ConfigureStoreOptions<
  S = any,
  A extends Action = AnyAction,
  M extends Middlewares<S> = Middlewares<S>
> {
  /**
   * A single reducer function that will be used as the root reducer, or an
   * object of slice reducers that will be passed to `combineReducers()`.
   */
  reducer: Reducer<S, A> | ReducersMapObject<S, A>

  /**
   * An array of Redux middleware to install. If not supplied, defaults to
   * the set of middleware returned by `getDefaultMiddleware()`.
   */
  middleware?: ((getDefaultMiddleware: CurriedGetDefaultMiddleware<S>) => M) | M

  /**
   * Whether to enable Redux DevTools integration. Defaults to `true`.
   *
   * Additional configuration can be done by passing Redux DevTools options
   */
  devTools?: boolean | DevToolsOptions

  /**
   * The initial state, same as Redux's createStore.
   * You may optionally specify it to hydrate the state
   * from the server in universal apps, or to restore a previously serialized
   * user session. If you use `combineReducers()` to produce the root reducer
   * function (either directly or indirectly by passing an object as `reducer`),
   * this must be an object with the same shape as the reducer map keys.
   */
  preloadedState?: DeepPartial<S extends any ? S : S>

  /**
   * The store enhancers to apply. See Redux's `createStore()`.
   * All enhancers will be included before the DevTools Extension enhancer.
   * If you need to customize the order of enhancers, supply a callback
   * function that will receive the original array (ie, `[applyMiddleware]`),
   * and should return a new array (such as `[applyMiddleware, offline]`).
   * If you only need to add middleware, you can use the `middleware` parameter instead.
   */
  enhancers?: StoreEnhancer[] | ConfigureEnhancersCallback
}

function configureStore<S = any, A extends Action = AnyAction>(
  options: ConfigureStoreOptions<S, A>
): EnhancedStore<S, A>
```
- `reducer`
  - これが単一の関数である場合、storeのroot reducerとして直接使用されます。
  - もしこれが `{users : usersReducer, posts : postsReducer}` のようなslice reducerのオブジェクトであれば、  
  configureStore はこのオブジェクトを `Redux combineReducers` ユーティリティに渡してルートリデューサーを自動的に作成します。
- `middleware`
  - このオプションを指定する場合、ストアに追加したいすべてのミドルウェア関数が含まれている必要があります。configureStoreは自動的にこれらの関数をapplyMiddlewareに渡します。
  - 指定しなかった場合、configureStore は getDefaultMiddleware をコールし、それが返すミドルウェア関数の配列を使用します。  
- `devTools`
  - configureStoreが自動的にRedux DevToolsブラウザ拡張のサポートを有効にするかどうかを示す
  - デフォルトは true 
  ```ts
  devTools: process.env.NODE_ENV !== 'production',
  ```
- `preloadedState`
  - ReduxのcreateStore関数に渡す、オプションの初期状態の値
- `enhancers`
  - Redux自体に機能を追加するための正式なメカニズム 

#### combineReducers(reducers)
combineReducersの処理の本筋部分だけを取り出すと

1. 各reducerを呼び出して初期状態を取り出す
2. 初期状態をまとめて初期状態ツリーを作る
3. reducerの処理をまとめたcombination関数を返す
4. combineReducersが返すcombination関数はreducer名をキーにした状態ツリーから、そのreducerの部分状態を取り出して渡す。  
  戻ってきた新しい部分状態はやはりreducer名をキーにしてまた１つの状態ツリーにまとめられて、最終的なreducerの戻り値となる。  

[Reduxにおけるreducer分割とcombineReducersについて](https://qiita.com/kuy/items/59c6d7029a10972cba78)  
[combineReducersでハマったメモ](https://qiita.com/usagi-f/items/ae568fb64c2eac882d05)  

#### getDefaultMiddleware
```ts
import { configureStore } from '@reduxjs/toolkit'

import logger from 'redux-logger'

import rootReducer from './reducer'

const store = configureStore({
  reducer: rootReducer,
  middleware: (getDefaultMiddleware) => getDefaultMiddleware().concat(logger),
})

// Store has all of the default middleware added, _plus_ the logger middleware
```
middlewareの登録に使用する  

### 2. Provide the Redux Store to React
```tsx
import React from 'react'
import ReactDOM from 'react-dom'
import './index.css'
import App from './App'
import { store } from './app/store'
import { Provider } from 'react-redux'

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

#### Provider [doc(Provider)](https://react-redux.js.org/api/provider)
`<Provider>`コンポーネントは、Reduxストアにアクセスする必要があるネストされたコンポーネントが、Reduxストアを利用できるようにする  
React Reduxアプリ内のどのReactコンポーネントもストアに接続できるため、ほとんどのアプリケーションではトップレベルで`<Provider>`をレンダリングし、その中にアプリのコンポーネントツリー全体が入るようにする  

#### Store [doc(Store)](https://redux.js.org/api/store)
storeはアプリケーションの全状態ツリーを保持する  
その中の状態を変更する唯一の方法は、その状態に対してactionをdispatchすること  
ストアはクラスではなく、いくつかのメソッドを持つ単なるオブジェクト  

Store Methods
- getState()
- dispatch(action)
- subscribe(listener)
- replaceReducer(nextReducer)

##### getState()
アプリケーションの現在の状態ツリーを返す。  
これは、ストアのreducerが最後に返した値と同じ  

返り値: (any): The current state tree of your application.

##### dispatch(action)
actionをdispatchする。これは状態変化のトリガーとなる唯一の方法である  
```ts
import { createStore } from 'redux'
const store = createStore(todos, ['Use Redux'])

function addTodo(text) {
  return {
    type: 'ADD_TODO',
    text
  }
}

store.dispatch(addTodo('Read the docs'))
store.dispatch(addTodo('Read about the middleware'))
```

##### subscribe(listener)
ステートを変更するのが自分であればステート変更のタイミングは分かりますが、誰か他の人が（=他の処理が）更新した時には、ステートがいつ変更されたのかわからない。  
他の人がステートを変更した際に、その変更を教えてもらうのが、サブスクライブ（subscribe）という機能  
**subscribe関数を使用することによってstateが変更された際に、引数に指定したlistener（関数）を実行することが可能**  
```ts
// 常に最新のstateを取得する例
// stateを変更するとsubscribe()の引数に指定されたコールバック関数が実行される
let appState = store.getState();
store.subscribe(() => {appState = store.getState()});
```
listener (Function): アクションがディスパッチされ、ステートツリーが変更されたときに呼び出されるコールバック  
このコールバックの内部でgetState()を呼び出すと、現在のステートツリーを読み出すことができる。  

##### replaceReducer(nextReducer)
replaceReducerは主に、reducerを動的に追加したり、ホットリロードを有効にしたりするために、使用される  
```ts
const store = createStore(plusMinus);

store.subscribe(() => console.log(store.getState()));

store.dispatch({type: INCREMENT, amount: 5}); // 5
store.dispatch({type: DECREMENT, amount: 3}); // 2

store.replaceReducer(multiplication); // 2

// reducer が multiplication に変わっているので INCREMENT は実行されず state がそのまま返される
store.dispatch({type: INCREMENT, amount: 4}); // 2

store.dispatch({type: MULTIPLICATION, amount: 3}); // 6
```
replaceReducerを実行してもstateはそのまま維持される。これを理解せずに実装を行うと、不具合を生んでしまう可能性がある  

[store.replaceReducer で reducer を入れ替える](https://numb86-tech.hatenablog.com/entry/2020/04/12/185515)  

### 3. Create a Redux State Slice
src/features/counter/counterSlice.ts  

```tsx
import { createSlice, PayloadAction } from '@reduxjs/toolkit'

export interface CounterState {
  value: number
}

const initialState: CounterState = {
  value: 0,
}

export const counterSlice = createSlice({
  name: 'counter',
  initialState,
  reducers: {
    increment: (state) => {
      // Redux Toolkit allows us to write "mutating" logic in reducers. It
      // doesn't actually mutate the state because it uses the Immer library,
      // which detects changes to a "draft state" and produces a brand new
      // immutable state based off those changes
      state.value += 1
    },
    decrement: (state) => {
      state.value -= 1
    },
    incrementByAmount: (state, action: PayloadAction<number>) => {
      state.value += action.payload
    },
  },
})

// Action creators are generated for each case reducer function
export const { increment, decrement, incrementByAmount } = counterSlice.actions

export default counterSlice.reducer
```
スライスを作成するには、スライスを識別するための`name`、ステートの`initialState`、ステートの更新方法を定義するための1つ以上の`rdeducer`関数が必要です。  
スライスが作成されると、生成されたReduxアクションクリエイターとスライス全体のレデューサー関数をエクスポートすることができます。   

Reduxでは、データのコピーを作成し、そのコピーを更新することで、すべての状態の更新をイミュータブルに記述する必要があります。  
しかし、Redux ToolkitのcreateSliceとcreateReducerのAPIは内部でImerを使って、正しいimmutable更新となる「mutating」更新ロジックを書くことができるようになっています。  

immutableとは：　作成後にその状態を変えることのできないオブジェクトのこと  

#### createSlice
slice というのは簡単に言うと reducer 関数と action creator を含むオブジェクト  
createActionとcreateReducerを使わなくても、createSliceを使ったら reducer を作成するだけで自動的に action type も定義してくれるし action creator も生成してくれる  
ちなみに "slice" という名前になったのは「combineReducerでまとめられた root reducer を構成する1つのスライス(薄片)だから」という理由  

createSliceの型  
```ts
function createSlice({
    //名称（action type に使われる）
    name: string,
    //reducer の初期値
    initialState: any,
    //case reducer たちのオブジェクト
    reducers: Object<string, ReducerFunction | ReducerAndPrepareObject>
    //別で作成しておいた action に対する reducer（任意）
    extraReducers?:
    | Object<string, ReducerFunction>
    | ((builder: ActionReducerMapBuilder<State>) => void)
})
```

createSliceの返り値  
```ts
{
    name : string,
    reducer : ReducerFunction,
    actions : Record<string, ActionCreator>,
    caseReducers: Record<string, CaseReducer>
}
```

