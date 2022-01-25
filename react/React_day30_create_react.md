## やったこと
reactの環境構築についての勉強

## webpackについて
![スクリーンショット 2022-01-25 10 08 08](https://user-images.githubusercontent.com/78260526/150891543-3921c733-c435-4ba0-aa20-8cdd6a79bab0.png)  
複数のファイルを１つにまとめて出力してくれるツール  
（複数ファイルをまとめることを「バンドル」と呼ぶ）  

メリット
- コードが読みやすくなる
- 保守性が高くなる
- コードを他プロジェクトに転用しやすくなる
- JSだけでなく、CSSや画像もバンドルできる
- 包括的な開発環境が整う（開発作業の分担やテストがしやすくなる）

### 現代フロントエンドに欠かせないwebpackとBabelを理解しよう！ builderscon tokyo 2019
JSにはECMAScriptという言語仕様がある  
この仕様を決めているのがTC39委員会  

#### Babelとは?
JSのコンパイラ  
元々は6to5という名前だった  
- ES6からES5へのコードを変換する機能しか持ってなかった
- しかし、それ以外の機能も必要なため名前が合わない -> Babelに

使用できるJSはブラウザによって異なるためBabelが必要になる  
これによって新しいJSを各ブラウザで使用できる  

#### Babelの機能
- 構文変換
- ソースコードの変換(codemods)
- ターゲットブラウザにない環境のPolyfillの提供

#### コードをどのように変換するのか？
1. **Parsing**: @babel/parser
  - ソースコードを**Abstract Syntax Tree(AST)**に変換する
2. **Transformation**
  - ASTを変換するBabelのプラグインがこの役割を担っている
3. **Code Generat**: @babel/generator
  - ASTをソースコードに変換する

<img src="https://user-images.githubusercontent.com/78260526/150894471-528c2d7c-942d-424d-a785-c9e644142f0d.png" width="500px" />
<img src="https://user-images.githubusercontent.com/78260526/150894346-e452dc5b-1735-4e31-b5dd-422e94bb2841.png" width="500px" />

#### webpackとは？
JSだけでなく、img, cssなどもモジュール化する  
依存関係も解決する  

#### webpackの誕生
CommonJSをブラウザ用に変換する「modules-webmake」があった  
これに対してCode Splitting機能を追加する提案をしたが受け入れられなかったので、勝手にforkして開発したのがwebpack  

#### webpackのコンセプト
- **Entry**
  - 起点となるファイルを指定
- **Output**
  - 処理が完了したファイルを出力する
- **Loaders**
  - 指定ファイルに任意の処理を加える、1つのタスク
- **Plugins**
  - webpackのさまざまなイベントをhookし、データを操作したり処理を実行する
- **Mode**
  - none, development, productionを指定してmodeに応じてパフォーマンス最適化や圧縮などを行う
- **Browser Compatibility**
  - ブラウザの後方互換性

#### 実行
1. ./bin/webpack.jsを実行
2. その中で./bin/webpack-cliを実行
3. webpack.comfig.jsを読み込む

<img src="https://user-images.githubusercontent.com/78260526/150897369-429ed798-59c3-46ed-ace1-098d7e09a1de.png" width="500px" />

参考動画 [現代フロントエンドに欠かせないwebpackとBabelを理解しよう！ (__sakito__) - builderscon tokyo 2019](https://www.youtube.com/watch?v=gWzf-BEhTWk)  

## さぁ何はともあれwebpackの公式を読もう
### Concepts
- Entry
- Output
- Loaders
- Plugins
- Mode
- Browser Compatibility

### Entry Points
#### Single Entry (Shorthand) Syntax:   
`Usage: entry: string | [string]`  
```js
module.exports = {
  entry: './path/to/my/entry/file.js',
};
```
entryを設定していない場合、webpackは  
./src/index.jsを./dist/main.jsに出力する  
という挙動になる  

細かく書くと、

1. ./src/index.jsをエントリーポイントとし
2. index.js内でimport、requireされ使われているファイルをバンドルして
3. ./dist/main.jsに出力する
という動き

[webpackの基本だけどハマりやすいentryの設定と[name]](https://qiita.com/sansaisoba/items/921438a19cbf5a31ec53)  


### Output
```js
module.exports = {
  output: {
    filename: 'bundle.js',
  },
};
```

複数のEntrypointの場合
```js
module.exports = {
  entry: {
    app: './src/app.js',
    search: './src/search.js',
  },
  output: {
    filename: '[name].js',
    path: __dirname + '/dist',
  },
};

// writes to disk: ./dist/app.js, ./dist/search.js
```

### mode
mode プロパティのパラメーターを development、production、または none に設定することにより各環境に対応する最適化を有効にする  
- development：開発時向けのオプション（ソースコードが読みやすい状態で出力）
- production：本番環境（公開時）向けのオプション（ソースコードを圧縮及び最適化）
- none: 最適化を行わない


### Loaders
説明をdeepLにかけてみた  
> ローダーとは、モジュールのソースコードに適用される変換のことです。これを使うと、ファイルをインポートしたり「ロード」したりするときに、前処理をすることができます。このように、ローダーは他のビルドツールにおける「タスク」のようなもので、フロントエンドのビルドステップを処理するための強力な方法を提供します。ローダーは、異なる言語（TypeScriptなど）のファイルをJavaScriptに変換したり、インライン画像をデータURLとして読み込んだりすることができます。ローダーは、JavaScriptモジュールからCSSファイルを直接インポートするようなことも可能です!

#### Loadersとは
webpackはJavaScriptファイルのみそのままの状態で取り扱うことができる。  
しかしJavaScriptファイル以外の他の言語で書かれたプログラムをwebpackで扱うことができない。  
JavaScript以外のファイルでもwebpackでも扱えるようにする場合にはLoader(ローダー)を利用する必要がある。  
Loaderは一つではなくそれぞれの言語や行いたい処理に対応するLoaderが存在する。  

#### typescriptとts-loader
[ts-loader公式](https://github.com/TypeStrong/ts-loader#getting-started)  
```ts
module: {
  rules: [
    {
      test: /\.ts$/,            // 拡張子 .ts のファイルを
      use: 'ts-loader',         // ts-loaderでトランスパイルする
      exclude: /node_modules/,  // ただし外部ライブラリは除く
    },
  ],
},
```
ローダーの設定は module プロパティの `rules` プロパティで `test` と `use` の2つのプロパティを使って設定する  

[入門者/初心者にもわかるwebpack5の基礎(Loader編)](https://reffect.co.jp/html/webpack-loader-setting-for-beginner#i)  

### Plugins

#### webpackでESLintが使える環境を構築してみる
```
$ npm install --save-dev eslint eslint-webpack-plugin
```
「eslint-webpack-plugin」のパッケージをインストール  

```js
const ESLintPlugin = require('eslint-webpack-plugin');

module.exports = {
  // ...
  plugins: [new ESLintPlugin(options)],
  // ...
};
```
そしてwebpackの設定に追加する  

### context
タイプ： String  
Default: compiler.context  
ファイルのルートを示す文字列。  

### devtool ソースマップ
devtool オプションを使うと出力されるソースマップを設定することができる  
devtool オプションにソースマップのタイプを指定してビルドすると、出力ファイルと同じディレクトリに出力ファイルと同じ名前で拡張子が「.js.map」のソースマップファイルが作成される  
ビルドするとdistフォルダにmain.js.mapが作成される  

```js
dist
├── index.html
├── main.js
└── main.js.map //ソースマップファイル
```

### resolve
webpack には import を使ってモジュールをインポートする際に、指定されたモジュールを検索して該当するファイルを探す仕組みがある  
resolve オプションはモジュール解決（モジュールの import を解決する仕組み）の設定を変更する  
#### resolve.modules
モジュールを解決するときに検索するディレクトリを webpack に指示する  
```js
module.exports = {
  //...
  resolve: {
    modules: ['node_modules']
  }
};
```

#### resolve.extentions
このオプションで指定されている拡張子のファイルは import の際に拡張子を省略することができる  
```js
      extensions: [".ts", ".tsx", ".js"],
```
この設定をするとこれらのimport時に拡張子を省略できる  






