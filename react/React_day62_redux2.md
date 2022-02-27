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
entityを常にsortしてくれるパラメータもある
```tsx
const booksAdapter = createEntityAdapter<Book>({
  // Keep the "all IDs" array sorted based on book titles
  sortComparer: (a, b) => a.title.localeCompare(b.title),
})
```

#### CRUD Functions
- `addOne`: accepts a single entity, and adds it if it's not already present.
- `addMany`: accepts an array of entities or an object in the shape of Record<EntityId, T>, and adds them if not already present.
- `setOne`: accepts a single entity and adds or replaces it
- `setMany`: accepts an array of entities or an an object in the shape of Record<EntityId, T>, and adds or replaces them.
- `setAll`: accepts an array of entities or an object in the shape of Record<EntityId, T>, and replaces all existing entities with the values in the array.
- `removeOne`: accepts a single entity ID value, and removes the entity with that ID if it exists.
- `removeMany`: accepts an array of entity ID values, and removes each entity with those IDs if they exist.
- `removeAll`: removes all entities from the entity state object.
- `updateOne`: accepts an "update object" containing an entity ID and an object containing one or more new field values to update inside a changes field, and performs a shallow update on the corresponding entity.
- `updateMany`: accepts an array of update objects, and performs shallow updates on all corresponding entities.
- `upsertOne`: accepts a single entity. If an entity with that ID exists, it will perform a shallow update and the specified fields will be merged into the existing entity, with any matching fields overwriting the existing values. If the entity does not exist, it will be added.
- `upsertMany`: accepts an array of entities or an object in the shape of Record<EntityId, T> that will be shallowly upserted.

#### getInitialState
Returns a new entity state object like {ids: [], entities: {}}.  

#### Selector Functions
The entity adapter will contain a getSelectors() function that returns a set of selectors that know how to read the contents of an entity state object:

- `selectIds`: returns the state.ids array.
- `selectEntities`: returns the state.entities lookup table.
- `selectAll`: maps over the state.ids array, and returns an array of entities in the same order.
- `selectTotal`: returns the total number of entities being stored in this state.
- `selectById`: given the state and an entity ID, returns the entity with that ID or undefined.

### createAsyncThunk
sample usage
```tsx
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

#### ちょっと思った、payloadって何？
[What is a Payload in Redux context](https://stackoverflow.com/questions/51357412/what-is-a-payload-in-redux-context)  

Payload is what is keyed ( the key value pairs ) in your actions and passed around between reducers in your redux application. For example -

```tsx
const someAction = {
  type: "Test",
  payload: {user: "Test User", age: 25},
}
```

This is a generally accepted convention to have a type and a payload for an action

### Selector
[reselect](https://github.com/reduxjs/reselect)  
[Deriving Data with Selectors](https://redux.js.org/usage/deriving-data-selectors)  







