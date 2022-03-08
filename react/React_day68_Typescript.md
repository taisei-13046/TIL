## やったこと
Typescriptのdocumentを読む  
と思ったら全文英語でハードル高すぎる  
のでサバイバルTypescriptに変更  

## [サバイバルTypeScript](https://typescriptbook.jp/)  
### Typescript エコシステム
TypeScriptはJavaScriptにコンパイルする「トランスパイラ」と呼ばれる技術です。トランスパイラはTypeScriptの他に、CoffeeScriptやDartがあります。技術的にTypeScriptもCoffeeScriptも同じジャンルですが、CoffeeScriptやDartがまったく新しい言語をJavaScript環境で動かせるようにするためにトランスパイルを採用しているのに対し、TypeScriptはあくまでJavaScriptプラスアルファなコードをJavaScript環境で動かせるようにするためにトランスパイルをするという点で、やや毛色が異なります。

トランスパイラの一種にBabelというツールがあります。これはJavaScriptの最新の言語仕様を、古いJavaScript環境でも使えるように変換するものです。BabelはTypeScriptと異なり、型のチェックなどは行いません。  

### JavaScriptとECMAScriptの関係
ECMAScriptはJavaScriptの仕様を定義したものです。仕様とは決まりごとのことで、ブラウザなどがJavaScriptを読み込んだときに、どのような文法を解釈しなければならないか、処理がどのように動くべきかといったことを決めたものです。ECMAScriptという異なる名前がついていますが、JavaScriptと別の言語があるわけではありません。  

ECMAScriptは毎年1回、仕様改定されます。改定されるごとにバージョンが上がります。ECMAScriptのバージョンはリリースされた西暦になってます。たとえば、2021年に改定されたECMAScriptはES2021となります。TypeScriptもECMAScriptの仕様改定に合わせて、アップデートされていきます。  

#### ECMAScriptとブラウザの仕様
JavaScriptのうちブラウザ仕様に関する部分は、HTML Living Standardが決めています。ブラウザでJavaScriptを使うと、触れることになるのがwindowオブジェクトやHTMLDivElement、ローカルストレージなどのAPIです。これらはHTML Living Standardと呼ばれる規格が定めています。この規格はEcmaインターナショナルとは異なる標準化団体WHATWGが策定しています。

JavaScriptの機能の中でも、ECMAScriptとHTML Living Standardで役割分担があるものもあります。たとえばモジュールです。ECMAScriptはモジュールの仕様を定めます。importやexportの構文や、モジュール内部の仕様などは、ECMAScriptが定めます。一方、モジュールの具体的なロード方法はHTML Living Standardが定めています。たとえば、import "指定子";の指定子の部分にどんな文字列を書いていいか、モジュールはどの順番でロードするかなどはHTML Living Standardが定めます。  

#### ECMAScriptとブラウザの関係性
主要なブラウザの内部を分解すると、レンダリングエンジンやJavaScriptエンジンと呼ばれる部品の単位があります。

JavaScriptエンジンは、ECMAScriptを実装したモジュールです。JavaScriptエンジンには、主要なものでV8、SpiderMonkey、JavaScriptCoreがあります。

レンダリングエンジンは、JavaScriptエンジンを組み込んだブラウザの表示機能を担うモジュールです。有名なレンダリングエンジンは、Blink、Gecko、WebKitがあります。たとえば、BlinkはV8をJavaScriptエンジンに採用しています。レンダリングエンジンはJavaScriptだけでなく、HTMLやCSSを解釈し、画面描画を総合的に行います。  

![browser-rendering-engine-javascript-engine-ecmascript-relations-977f2898337ec4fefb4fa76955d63913](https://user-images.githubusercontent.com/78260526/157189149-a2b384dc-f383-48ff-8444-79d933b2f1b5.svg)

### varはもう使わない
#### varの問題点
1. 同名の変数宣言

```js
function test() {
  var x = 1;
  var x = 2;
  console.log(x);
}
```

2. グローバル変数の上書き

varはグローバル変数として定義されたときに、windowオブジェクトのプロパティとして定義されるため、既存のプロパティを上書きする危険性があります。

たとえば、ブラウザ上でinnerWidth変数をグローバル変数として定義してしまうと、標準APIのwindow.innerWidthが上書きされるため、ブラウザの幅を変更しても常に同じ値が返ってくるようになってしまいます。  

```js
var innerWidth = 10;
console.log(window.innerWidth);
10
```

3. 変数の巻き上げ
JavaScriptで宣言された変数はスコープの先頭で変数が生成されます。これは変数の巻き上げと呼ばれています。varで宣言された変数は、スコープの先頭で生成されてundefinedで値が初期化されます。次の例ではgreeting変数への参照はエラーとならずにundefinedとなります。  

```js
console.log(greeting);
var greeting = "こんにちは";
 
// ↓ 巻き上げの影響で実際はこう実行される
 
var greeting;
console.log(greeting);
greeting = "こんにちは";
```

ただ、ここで注意すべきなのがletとconstの場合でも変数の巻き上げは発生しているという点です。では、なぜReference Errorが発生するのでしょうか？

varは変数の巻き上げが発生したタイミングでundefinedで変数を初期化しているため、値の参照が可能となっていました。それに対してletとconstは変数の巻き上げが発生しても変数が評価されるまで変数は初期化されません。そのため、初期化されていない変数を参照するためReference Errorが発生しているのです。  

### プリミティブ型 (primitive types)
JavaScriptのデータ型は、プリミティブ型とオブジェクトの2つに分類されます。

JavaScriptのプリミティブ型の1つ目の特徴は、値を直接変更できない点です。つまりイミュータブル(immutable)です。一方、オブジェクトには、値を後で変更できるというミュータブル特性(mutable)があります。  

プリミティブ型は次の7つがあります。
1. 論理型(boolean): trueまたはfalseの真偽値。
2. 数値型(number): 0や0.1のような数値。
3. 文字列型(string): "Hello World"のような文字列。
4. undefined型: 値が未定義であることを表す型。
5. ヌル型(null): 値がないことを表す型。
6. シンボル型(symbol): 一意で不変の値。
7. bigint型: 9007199254740992nのようなnumber型では扱えない大きな整数型。

上のプリミティブ型以外は、JavaScriptにおいてはすべてオブジェクトと考えて問題ありません。配列や正規表現オブジェクトなどもすべてオブジェクトです。

### 数値型 (number type)
#### 数値の区切り文字(numeric separators)
JavaScriptの数値リテラルは可読性のためにアンダースコアで区切って書けます。何桁ごとに区切るかは自由です。表したい値や、国と地域の慣習などに合わせて選択できます。

```js
100_000_000 // 1億
```

#### 数値リテラルのプロパティ
JavaScriptの数値リテラルのプロパティを直接参照する場合、小数点のドットとプロパティアクセッサーのドットが区別できないため、構文エラーになります。

```js
5.toString(); // この書き方は構文エラー
```

これを回避するには、ドットを2つ続けるか、数値をカッコで囲む必要があります。

```js
5..toString();
(5).toString();
```

#### 特殊な数値
JavaScriptの数値型には、NaNとInfinityという特殊な値があります。

**NaN**  

NaNは非数(not-a-number)を表す変数です。JavaScriptでは、処理の結果、数値にならない場合にNaNを返すことがあります。たとえば、文字列を数値に変換するparseInt関数は、数値化できない入力に対し、NaNを返します。

```js
const price = parseInt("百円");
console.log(price);
```

NaN
値がNaNであるかのチェックはNumber.isNaNを用います。

```js
const price = parseInt("百円");
if (Number.isNaN(price)) {
  console.log("数値化できません");
}
```

NaNは特殊で、等号比較では常にfalseになるので注意してください。

```js
console.log(NaN == NaN);
false
console.log(NaN === NaN);
false
```

Infinityは無限大を表す変数です。たとえば、1を0で割った場合、この値になります。


























