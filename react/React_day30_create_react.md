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

<img src="https://user-images.githubusercontent.com/78260526/150894279-e5162a90-d596-407c-802a-4baac1e89eef.png" width="500px" />




