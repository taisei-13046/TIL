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















