## やったこと
roadmapをメインに進めた

### レビュー
#### Atomでの定数の扱いについて
atomでwidth, heightを指定するのは柔軟性に欠けるため良くない。  
レイアウトに関するスタイルは親側で指定する  




### create react appの各ファイルについて
```
.
├── README.md
├── package.json
├── public
│   ├── favicon.ico
│   ├── index.html
│   ├── logo192.png
│   ├── logo512.png
│   ├── manifest.json
│   └── robots.txt
├── src
│   ├── App.css
│   ├── App.test.tsx
│   ├── App.tsx
│   ├── index.css
│   ├── index.tsx
│   ├── logo.svg
│   ├── react-app-env.d.ts
│   ├── reportWebVitals.ts
│   └── setupTests.ts
├── tsconfig.json
└── yarn.lock
``` 
特にわからなかったフォルダ  

#### react-app-env.d.ts
```ts
/// <reference types="react-scripts" />
```
これにはこの１行だけが書かれている  
`react-scripts`モジュール内の別のd.tsファイルへの参照が含まれています。  
d.tsファイルにはTypeScriptタイプ定義が含まれています。このreact-app.d.tsファイルは、さまざまなモジュールのタイプを定義します。  

#### reportWebVitals.ts
v4でWebVitalsの計測ライブラリがデフォルトで組み込まれるようになりました  

```ts
src/reportWebVitals.ts
import { ReportHandler } from 'web-vitals';

const reportWebVitals = (onPerfEntry?: ReportHandler) => {
  if (onPerfEntry && onPerfEntry instanceof Function) {
    import('web-vitals').then(({ getCLS, getFID, getFCP, getLCP, getTTFB }) => {
      getCLS(onPerfEntry);
      getFID(onPerfEntry);
      getFCP(onPerfEntry);
      getLCP(onPerfEntry);
      getTTFB(onPerfEntry);
    });
  }
};

export default reportWebVitals;
```

web-vitalsはGoogleChromeが提供するWebVitals計測ライブラリ  

##### 使い方[(公式)](https://create-react-app.dev/docs/measuring-performance/)
reportWebVitals.tsでexportされたreportWebVitalsを呼び出して引数に関数を渡して使います  
参考 [CreateReactAppにWebVitals計測ライブラリが入ったので試してみた](https://qiita.com/ozaki25/items/6139cbc70cf988d1c870)   


#### setupTests.ts
「create-react-app」におけるJestの設定ファイル  

### Reactのテストについて

#### テストの概要
React コンポーネントをテストするのにはいくつか方法がありますが、大きく 2 つのカテゴリに分けられます。  
- **コンポーネントツリーのレンダー** をシンプルなテスト環境で行い、その結果を検証する
- **アプリケーション全体の動作** をブラウザ同等の環境で検証する（end-to-end テストとして知られる）

このセクションでは、最初のケースにおけるテスト戦略にフォーカスします。end-to-end テストが重要な機能のリグレッションを防ぐのに有効である一方で、そのようなテストは React コンポーネントとは特に関係なく、このセクションのスコープ外です。


##### トレードオフ
**実行速度 vs 環境の現実度**： 変更を加えてから結果を見るまでのフィードバックが早いツールは、ブラウザでの動作を正確に模倣しません。一方実際のブラウザ環境を使うようなツールは、イテレーションの速度が落ちる上 CI サーバでは壊れやすいです。  
**モックの粒度** コンポーネントのテストでは、ユニットテストとインテグレーションテストの区別は曖昧です。フォームをテストする時、そのテストはフォーム内のボタンもテストすべきでしょうか。それともボタンコンポーネント自体が自身のテストを持つべきでしょうか。ボタンのリファクタリングはフォームのテストを壊さないべきでしょうか。  

##### 推奨ツール
**Jest** は jsdom を通じて DOM にアクセスできる JavaScript のテストランナーです。jsdom はブラウザの模倣環境にすぎませんが、React コンポーネントをテストするのには十分なことが多いです。   
**React Testing Library** は実装の詳細に依存せずに React コンポーネントをテストすることができるツールセットです。このアプローチはリファクタリングを容易にし、さらにアクセシビリティのベスト・プラクティスへと手向けてくれます。コンポーネントを children 抜きに「浅く」レンダーする方法は提供していませんが、Jest のようなテストランナーで モック することで可能です。  



[テストのレシピ集](https://ja.reactjs.org/docs/testing-recipes.html)   
テストにおいて、通常われわれは React ツリーを document に結びついた DOM 要素として描画することになります。これは DOM イベントを受け取れるようにするために重要です。テストが終了した際に「クリーンアップ」を行い、document からツリーをアンマウントします。  
このためによく行うのは beforeEach と afterEach ブロックのペアを使い、それらを常に実行することで、各テストの副作用がそれ自身にとどまるようにすることです。  
仮にテストが失敗した場合でもクリーンアップコードを実行するようにするべき、ということを覚えておいてください。さもなくば、テストは「穴の開いた」ものとなってしまい、あるテストが他のテストの挙動に影響するようになってしまいます。これはデバッグを困難にします。

#### `act()`  
UI テストを記述する際、レンダー、ユーザイベント、データの取得といったタスクはユーザインターフェースへのインタラクションの「ユニット (unit)」であると考えることができます。  
react-dom/test-utils が提供する act() というヘルパーは、あなたが何らかのアサーションを行う前に、これらの「ユニット」に関連する更新がすべて処理され、DOM に反映されていることを保証します。  
```tsx
act(() => {
  // render components
});
// make assertions
```

## JavaScript MDNを読む
[JavaScript](https://developer.mozilla.org/ja/docs/Web/JavaScript)  
JavaScript (JS) は軽量で、インタープリター型、あるいは実行時コンパイルされる、第一級関数を備えたプログラミング言語です。  
**第一級関数**とは、その言語の関数がその他の変数と同様に扱われることを表します。例えば、こうした言語では、関数を他の関数への引数として渡したり、他の関数から返却したり、変数の値として代入したりすることができます。    
[First-class Function (第一級関数)](https://developer.mozilla.org/ja/docs/Glossary/First-class_Function)  

### JavaScript とは
JavaScript のごく一般的な用途は、(先ほど例示した) Document Object Model API を介して動的に HTML と CSS を変更し、ユーザーインターフェイスを更新すること  

#### DOMとは
[DOM の紹介](https://developer.mozilla.org/ja/docs/Web/API/Document_Object_Model/Introduction)  
Document Object Model (DOM) は HTML や XML 文書のためのプログラミングインターフェイス  
ウェブページは文書です。この文書はブラウザーのウィンドウに表示されるか HTML ソースとして表示することが可能です。しかし両方の場合においてもそれは同じ文書です。ドキュメントオブジェクトモデル (DOM) は、その同じ文書を表現、保存、操作する方法です。DOM はウェブページの完全なオブジェクト指向の表現で、 JavaScript のようなスクリプト言語から変更できます。    

当初、 JavaScript と DOM は密接に絡み合っていましたが、最終的には別々の存在に進化しました。ページの内容は DOM に格納されており、 JavaScript を介してアクセスしたり操作したりすることができるので、この近似式を書くことができます。  

`**API = DOM + JavaScript**`  

#### DOM インタフェース
多くのオブジェクトは複数のインターフェイスを受けついでいます。例えば、 table オブジェクトでは、特別な HTMLTableElement インターフェイスを実装しており、そのインターフェイスは createCaption や insertRow などのメソッドを含んでいます。しかし、 table は HTML の要素でもあるので、 DOM の Element リファレンスの章で説明している Element インターフェイスも実装しています。さらには、 HTML の要素は、 DOM を考慮する限り、ウェブページや XML ページのオブジェクトモデルを作りあげるノードのツリー内にあるノードもであるので、 table オブジェクトはより基本的な Node インターフェイスを、 Element から継承して実装しています。  

#### JavaScript の実行順序
ブラウザーが JavaScript のブロックを見つけたとき、たいていは先頭から最後に向かって順番に実行されます。  

#### インタープリターとコンパイルコード
- インタープリターでは、コードが上から下に実行されてコードの実行結果がすぐに返ってきます。ブラウザーが実行する前にコードを何らかの形に変換する必要はありません。コードはプログラマーに親しみやすいテキストで受け取って、それから直接処理されます。  
- コンパイル言語はコンピューターで実行する前に他の形式に変換 (コンパイル) しなければなりません。例えば C/C++ は機械語にコンパイルされてから、コンピューターで実行されます。プログラムは、元のプログラムソースコードから生成されるバイナリーフォーマットで実行されます。

JavaScript は軽量なインタープリター型プログラミング言語です。ウェブブラウザーは元のテキストの形で JavaScript コードを受け取り、それからスクリプトを実行します。技術的な見地からは、たいていの JavaScript インタープリターは just-in-time compiling というテクニックを使ってパフォーマンスを向上させています  

#### スクリプトの読み込み方針
一般的な問題は、ページ上のすべての HTML が、現れた順に読み込まれることです。JavaScript を使用してページ上の要素（またはより正確には、Document Object Model）を操作している場合、何かをしようとする HTML の前に JavaScript が読み込まれ、解析されてもコードは機能しません。  

この問題に対する昔ながらの解決策は、すべての HTML が解析された後に読み込まれるように、body の下部に（たとえば `</body>` タグの直前に）script 要素を置くことでした。この解決策（および上記の DOMContentLoaded による解決策）の問題点は、HTML DOM が読み込まれるまでスクリプトの読み込みと解析が完全にブロックされることです。JavaScript がたくさんある大規模なサイトでは、これは大きなパフォーマンス上の問題を引き起こす可能性があり、サイトを遅くします。

実際には、スクリプトのブロッキングの問題を回避できるモダンな機能が 2 つあります — **async** と **defer**  
async 属性を使って読み込むスクリプトは(下記を見てください)、ページのレンダリングをブロックせずにスクリプトをダウンロードし、スクリプトのダウンロードが終了すると直ちに実行します。複数のスクリプトが特定の順序で実行されるという保証はありませんが、ページの残りの部分の表示を停止することはありません。  

#### 何が間違っている? JavaScript のトラブルシューティング
- **構文エラー**: プログラムが全く動かなかったり、途中で止まったりするような記述エラーで、通常はエラーメッセージが出力されます。正しいツールに慣れて、エラーメッセージの内容がわかるのなら、さほど無理なく修正が可能です。
- **論理エラー**: 書き方は正しくても、コードが意図した通りに動ないエラーです。つまりプログラムは動くのですが、間違った結果を返します。たいていの場合に、問題となる箇所に直接のエラーメッセージが出ることがないため、構文エラーよりも直すのが難しいことが多いです。

#### var と let の違い
その理由はやや歴史的なものです。JavaScript が最初に作成されたときには、var しかありませんでした。ほとんどの場合、基本的にはうまく機能しますが、その機能にはいくつかの問題があります。その設計は時々混乱したり、実に迷惑になることがあります。そこで、let を最新のバージョンの JavaScript で作成しました。これは、var とは多少異なる動作をする変数を作成するための新しいキーワードで、その過程での問題を修正しています。  

##### [varの巻き上げ](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Statements/var#var_hoisting)
変数の宣言 (および一般的な宣言) はコードを実行する前に処理されますので、変数はコード内のどこで宣言しても、コードの先頭で宣言したものと等価になります。  
JavaScriptでは、関数内で宣言されたローカル変数は、すべてその関数の先頭で宣言されたものとみなされる。
```js
bla = 2;
var bla;

// 次のように見なされます。

var bla;
bla = 2;
```
巻き上げはもはや let で動作しません。  
```js
var myname = "global";
 
function func() {
    console.log(myname);    //出力内容は？
    var myname = "local";
    console.log(myname);    //出力内容は？
}
 
func();
```
最初のconsole.logが”undefined“、次のconsole.logが”local“  

var を使用するとき、好きなだけ同じ変数を何度でも宣言することができます、しかし let ではできません。  
```js
var myName = 'Chris';
var myName = 'Bob';
```
varは重複宣言ができるが、letはエラーになる
```js
// error
let myName = 'Chris';
let myName = 'Bob';

// correct
let myName = 'Chris';
myName = 'Bob';
```

### Object のプロトタイプ
プロトタイプは、JavaScript オブジェクトが互いに機能を継承するメカニズムです。  
JavaScript はしばしばプロトタイプベースの言語として記述されます - 継承機能を提供するため、オブジェクトは prototype オブジェクト を持つことができます。これはテンプレートオブジェクトとして機能し、そこからメソッドやプロパティを継承します。

JavaScript では、あるオブジェクトのインスタンスとそのプロトタイプ (コンストラクタの prototype プロパティから派生した __proto__ プロパティ) の間にリンクが張られており、そのプロパティとメソッドはプロトタイプの連鎖を辿って発見されます。  

#### [プロトタイプオブジェクト](https://jsprimer.net/basic/prototype-object/)  
Objectには、他のArray、String、Functionなどのオブジェクトとは異なる特徴があります。 それは、他のオブジェクトはすべてObjectを継承しているという点です。  
正確には、ほとんどすべてのオブジェクトはObject.prototypeプロパティに定義されたprototypeオブジェクトを継承しています。   

![スクリーンショット 2022-02-05 15 06 04](https://user-images.githubusercontent.com/78260526/152630813-55c59916-e7c4-439c-a86d-55c44f58885b.png)  
このようなprototypeオブジェクトに組み込まれているメソッドはプロトタイプメソッドと呼ばれます。  

ObjectとObject.prototypeの関係と同じように、ビルトインオブジェクトArrayもArray.prototypeを持っています。 同じように、配列（Array）のインスタンスはArray.prototypeを継承します。 さらに、Array.prototypeはObject.prototypeを継承しているため、ArrayのインスタンスはObject.prototypeも継承しています。

`Arrayのインスタンス → Array.prototype → Object.prototype`  

### JSON の操作
JavaScript Object Notation (JSON) は、構造化データを表現するための標準のテキストベースの形式で、 JavaScript のオブジェクト構文に基づいています。ウェブアプリケーションでデータを転送する場合によく使われます  
JSON は JavaScript オブジェクトの構文に従ったテキストベースのデータ形式で、Douglas Crockford によって普及されました。JSON は JavaScript オブジェクトの構文に似ていますが、 JavaScript とは独立して扱われることがあり、多くのプログラミング言語環境には JSON を読み取ったり（解釈したり）生成したりする機能があります。  

#### 注意点
- JSON は指定されたデータ形式の純粋な文字列です。プロパティのみを含むことができ、メソッドを含むことはできません。 
- JSON では文字列とプロパティ名を二重引用符で括る必要があります。単一引用符は、JSON 文字列全体を囲む以外では無効です。
- カンマやコロンが 1 つ抜けるだけでも JSON ファイルは無効になり、動作しません。利用しようとしているデータを注意して確認してください















