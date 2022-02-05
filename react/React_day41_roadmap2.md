## やったこと
roadmapをメインに進めた

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
Document Object Model (DOM) は HTML や XML 文書のためのプログラミングインターフェイス  











