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

#### configureSotre

