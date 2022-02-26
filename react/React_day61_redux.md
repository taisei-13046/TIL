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









