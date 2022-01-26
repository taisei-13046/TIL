## やったこと
Redux Toolkitについて

### Quick Start
Install Redux Toolkit and React-Redux
```
$ npm install @reduxjs/toolkit react-redux
```
1. Create a Redux Store

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



