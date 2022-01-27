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



