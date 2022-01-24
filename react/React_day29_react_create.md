## やったこと
Reactの環境開発について学んだ  

## huskyについて
huskyとは  
**gitコマンドをhookにして、指定したコマンドを走られることができるもの**  
- npmパッケージ
- コミットやプッシュ時に、任意のコマンドを自動で実行できる
- リントやテストコマンドと組み合わせて使う

これによって手動でリントコマンドを打つ必要がなくなる  
[公式Doc](https://typicode.github.io/husky/#/)  

#### lint-stagedとの組み合わせ
lint-stagedと組み合わせることで、「git commit時にはステージング中のファイルに対してのみ」「git push時にはcommitしたファイルに対してのみ」コマンドを実行することができる  

GitのStagedされた状態とはgit addコマンドで修正されたファイルをコミットするため追加をした状態を意味する  
Staged状態のファイルは再び修正をするとgit addをしてまた追加する必要がある  
lint-stagedはStagedされたファイルを修正した後、git addをしなくても問題ないようにするツール  

#### husky & lint-staged で CI を実行する
**CIとは？**  
継続的インテグレーション（CI）とは、ソフトウェア開発においてソースコードの品質や保守性を保ち、持続可能な開発を続けていくための方法  
つまり、ほかの人が見ても理解しやすく、また自分が書いた古いコードでも処理内容が理解できるようにプログラミングしていくということ  

CI環境の導入
1. npm で husky と lint-staged、prettier、eslint をインストール
2. .huskyrc.json を用意し、pre-commit のフックに lint-staged を指定する設定を書く
3. .lintstagedrc.json を用意し、ファイルごとに実行する CI を定義


参考資料
- [qiita](https://qiita.com/shin4488/items/0a8013cc5e455f6fb25a#lint-staged%E3%81%A8%E3%81%AE%E7%B5%84%E3%81%BF%E5%90%88%E3%82%8F%E3%81%9B)
- [[React] husky, lint-staged](https://dev-yakuza.posstree.com/react/husky-lint-staged/)
- [lint-staged公式](https://github.com/okonet/lint-staged)
- [husky & lint-staged で CI を実行する](https://b-hood.site/articles/husky/#section-1)


## ESLintについて
ESLintの設定方法  
- 設定用のファイルとしてプロジェクトのルートディレクトリーに .eslintrc.yml もしくは .eslintrc.json を作成   
  - ESLint は `.eslintrc.*` もしくは `package.json` 内の `eslintConfig` 設定を読んでくれる  
- `.eslintrc.js` / `.eslintrc.cjs` は設定を動的に生成したいときに使用

### ESLint と Prettier を VS Code に導入し、設定する方法
ESLint 単体でもコード整形は可能ですが、Prettier の方がコード整形が優れているため、併用  
具体的には以下の点が優れている。
- ESLint では整形できないコードを整形できる
- ESLint と比べて手軽で確実に整形できる

Prettier を利用すべきだが、Prettier はコードフォーマッタのため、ESLint のような構文チェック機能はない  
そのため、コードの整形は Prettier が行い、コードの構文チェックは ESLint が行うように併用する  

#### 併用に必要なパッケージのインストール
```
npm install eslint@7.26.0 eslint-config-prettier@8.3.0 --save-dev
```
- eslint（ESLint 本体）
- eslint-config-prettier（ESLint のフォーマット関連のルールを全て無効にする、要は Prettier が整形した箇所に関してエラーを出さなくなる）

[git commit時に Prettier と ESLint が実行されるようにする](https://qiita.com/soarflat/items/06377f3b96964964a65d#git-commit%E6%99%82%E3%81%AB-prettier-%E3%81%A8-eslint-%E3%81%8C%E5%AE%9F%E8%A1%8C%E3%81%95%E3%82%8C%E3%82%8B%E3%82%88%E3%81%86%E3%81%AB%E3%81%99%E3%82%8B)  


## Reduxのディレクトリ構成
採用するのはre-ducksパターンだが、そのもとになるducksパターンを抑える
### Ducksパターン
Ducks パターンが解決すること： actionType、action、reducerの散らばり  
```markdown
// 一般的なディレクトリ構造

actions
├ articles.js
├ comments.js
└ users.js

reducers
├ articles.js
├ comments.js
└ users.js

types
├ articles.js
├ comments.js
└ users.js


// ducksパターン

modules
├ articles.js
├ comments.js
└ users.js
```
[ducks公式](https://github.com/erikras/ducks-modular-redux)  
ducksパターンでは今までactions, reducers, typesに分けられていたファイルを一つにする  
```ts
// widget.js

// Actions
const LOAD   = 'my-app/widgets/LOAD';
const CREATE = 'my-app/widgets/CREATE';
const UPDATE = 'my-app/widgets/UPDATE';
const REMOVE = 'my-app/widgets/REMOVE';

// Reducer
export default function reducer(state = {}, action = {}) {
  switch (action.type) {
    // do reducer stuff
    default: return state;
  }
}

// Action Creators
export function loadWidgets() {
  return { type: LOAD };
}

export function createWidget(widget) {
  return { type: CREATE, widget };
}

export function updateWidget(widget) {
  return { type: UPDATE, widget };
}

export function removeWidget(widget) {
  return { type: REMOVE, widget };
}

// side effects, only as applicable
// e.g. thunks, epics, etc
export function getWidget () {
  return dispatch => get('/widget').then(widget => dispatch(updateWidget(widget)))
}
```

### re-ducksパターン
ducksパターンによってディレクトリが分散する問題は解決されたが、一つのmoduleファイルが肥大化していって可読性が下がる  
Re-ducksパターンが解決すること： ducksパターンにおける module の肥大化
```markdown
ducks
├ articles
│   ├ index.js
│   ├ types.js
│   ├ actions.js
│   ├ reducers.js
│   ├ operations.js
│   └ selecors.js
│
├ comments
│   ├ index.js
│   ├ types.js
│   ├ actions.js
│   ├ reducers.js
│   ├ operations.js
│   └ selecors.js
│
└ users
    ├ index.js
    ├ types.js
    ├ actions.js
    ├ reducers.js
    ├ operations.js
    └ selecors.js
```
Re-ducksパターンでは機能ごとにディレクトリを分け、その中で関数の種類ごとにファイルを分ける  
つまりducksパターンの「機能ごとの分類」と、一般的なReduxの「関数の種類ごとの分類」をいい塩梅で組み合わせている  
- ディレクトリ分けの基準になる「機能」は、見た目で分類するよりもデータで分類した方が無理がない
- アプリケーションの
