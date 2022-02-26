## やったこと
Reduxについて  

### re-ducksのパターンを学ぶ
reduxは現在、re-ducksパターンに則るのが一般的   

**[alexnm/re-ducks](https://github.com/alexnm/re-ducks)**  

#### duckパターンのおさらい
- MUST `export default` a function called reducer()
- MUST export its action creators as functions
- MUST have action types in the form npm-module-or-app/reducer/ACTION_TYPE
- MAY export its action types as UPPER_SNAKE_CASE, if an external reducer needs to listen for them, or if it is a published reusable library

和訳 ↓

- reducerを呼ぶ関数は`export default`を使う
- action createrは関数としてexportする
- moduleは action type を npm-module-or-app/reducer/ACTION_TYPE という形で持たなくてはならない
- action type が他のディレクトリで必要ならば UPPER_SNAKE_CASE の形で export できる。

#### ディレクトリ構成
```
duck/
├── actions.js
├── index.js
├── operations.js
├── reducers.js
├── selectors.js
├── tests.js
├── types.js
├── utils.js
```

#### General rules for a duck folder
- MUST contain the entire logic for handling only ONE concept in your app, ex: product, cart, session, etc.
- MUST have an index.js file that exports according to the original duck rules.
- MUST keep code with similar purpose in the same file, ex: reducers, selectors, actions, etc.
- MUST contain the tests related to the duck.

和訳 ↓

- アプリ内に含まれるすべてのロジックを一つのコンセプトで扱う必要がある
- ducksルールに則ってexportする`index.js`を用意する
- 共通する目的を持つコードはまとめる
- testを書く必要がある

#### Types
Let's start from defining the constants we will use as redux action types. In order to keep the naming simple, let's call the file `types.js`, because `constants.js` is a bit too generic.  

```ts
// You can use any convention you wish here, but the name should remain UPPER_SNAKE_CASE for consistency.

const QUACK = "app/duck/QUACK";
const SWIM = "app/duck/SWIM";

export {
    QUACK,
    SWIM
};
```

命名規則は自分で選択していいが、UPPER_SNAKE_CASEを使用するのが良い

#### Actions
It's important to be consistent when defining actions, so let's always export functions from this file, we don't care if the action needs any input from the outside to build the payload or not.

アクションを定義するときに一貫性を持たせることが重要なので、常にこのファイルから関数をエクスポートするようにしましょう。アクションがペイロードを構築するために外部からの入力を必要とするかどうかは気にしません。  

```ts
/* ACTION CREATOR FUNCTIONS
Put here the functions that return an action object that can be dispatched
HINT: Always use functions for consistency, don't export plain objects
*/

import * as types from "./types";

const quack = ( ) => ( {
    type: types.QUACK
} );

const swim = ( distance ) => ( {
    type: types.SWIM,
    payload: {
        distance
    }
} );

export {
    swim,
    quack
};
```

#### Operations
In a simple application, you can easily dispatch simple actions and use the reducers to manage the state. However, in a more complex app you need to use some sort of middleware to handle more complex interactions. In our case, we use redux-thunk.

The operations file define the interface for our duck. You can reason about it like this: 1 operation = X actions dispatched. This makes each operation function either a thunk in case it needs to dispatch multiple actions, or simply a link to an action already defined in actions.js.  

operations とは、1つ以上の action を組み合わせたもの。個別の action は単純なままに、必要に応じて複雑な operations を作成できる。redux-thunkなどのミドルウェアはここで使う。

```ts
import * as actions from "./actions";

// This is a link to an action defined in actions.js.
const simpleQuack = actions.quack;

// This is a thunk which dispatches multiple actions from actions.js
const complexQuack = ( distance ) => ( dispatch ) => {
    dispatch( actions.quack( ) ).then( ( ) => {
        dispatch( actions.swim( distance ) );
        dispatch( /* any action */ );
    } );
}

export {
    simpleQuack,
    complexQuack
};
```

#### Reducers
It's a good practice to keep your state shape in a comment above the reducers, just to have an overview.

In case the state shape is more complex, you should break the reducers into multiple smaller functions that deal with a slice of the state, then combine them at the end.  


#### Selectors
In case your state shape is more complex you need selectors in order to map parts of the state to your props or in order to derive some data for your components from the current state.

These are the functions like: getVisibleTodos, isUserAuthenticated, etc. that take the current app state and return some derived data.  

selectors とは、state から必要な値を算出する関数のこと。state をシンプルに保つために、既存の state から算出できる値はすべて Selectors 経由で取得する。  

#### Index
```ts
import reducer from "./reducers";

import * as duckSelectors from "./selectors";
import * as duckOperations from "./operations";
import * as duckTypes from "./types";

export {
    duckSelectors,
    duckOperations,
    duckTypes
};

export default reducer;
```






