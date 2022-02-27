## やったこと
reduxの流れを抑える  

### configureStore
reducerが必須のオプションで、それ以外のmiddleware, devTools, preloadedState, enhancersは任意  
reducer部分には、複数のreducerを従来のcombineReducerの要領でオブジェクト形式で組み合わせて入れることもできれば、単体のreducerを入れることもできる。  
middlewareは、何も渡さなければgetDefaultMiddleware()が適用される。  
RTKではdevToolsも合わせてついてくるので、出し分け制御もできる。

```ts
export default configureStore({
  reducer: {
    // ...
  },
  middleware: : {
    // ...
  },
  devTools: process.env.NODE_ENV !== 'production'
})
```

```ts
const reducer = {
  auth: auth.reducer,
  user: user.reducer,
}

const middleware = getDefaultMiddleware()

export const store = configureStore({
  reducer,
  middleware,
})
```

`store/index.tsx`でreducerをまとめてstore管理できるようにする  

```tsx
function App({ store }: Props) {
  return (
    <Provider store={store}>
      {children}
    </Provider>
  )
}
```

Providerにstoreを渡すことでreduxのstate管理ができるようになる  

### createEntityAdapter
[createEntityAdapter](https://redux-toolkit.js.org/api/createEntityAdapter)  
[Redux Toolkit の EntityAdapter をさわってみる](https://www.cyokodog.net/blog/redux-toolkit-entity-adapter/)  

エンティティ操作用のアダプターを生成してくれ、CRUD(create, read, update, delete)操作の機能を提供してくれる  

```tsx
const booksAdapter = createEntityAdapter<Book>({
  // Assume IDs are stored in a field other than `book.id`
  selectId: (book) => book.bookId,
  // Keep the "all IDs" array sorted based on book titles
  sortComparer: (a, b) => a.title.localeCompare(b.title),
})
```

createEntityAdapterでアダプターを生成し、アダプターのgetInitialState()で初期 state を得る  

```ts
interface Task {
  id: number;
  title: string;
  done: boolean;
}
const tasksAdapter = createEntityAdapter<Task>();
 
const taskInitialEntityState = tasksAdapter.getInitialState();
```

例えば、アダプターを使ってデータ２件追加すると、state は次のような状態になる  

```
{
  ids: [1, 2]
  entities: {
    {
      1: {id: 1, title: 'aaa', done: false},
      2: {id: 2, title: 'bbb', done: false}
    }
  }
}
```

#### sortComparer
```tsx
const booksAdapter = createEntityAdapter<Book>({
  // Keep the "all IDs" array sorted based on book titles
  sortComparer: (a, b) => a.title.localeCompare(b.title),
})
```










