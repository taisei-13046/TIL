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





