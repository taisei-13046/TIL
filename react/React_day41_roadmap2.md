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
[テストのレシピ集](https://ja.reactjs.org/docs/testing-recipes.html)   
テストにおいて、通常われわれは React ツリーを document に結びついた DOM 要素として描画することになります。これは DOM イベントを受け取れるようにするために重要です。テストが終了した際に「クリーンアップ」を行い、document からツリーをアンマウントします。  
このためによく行うのは beforeEach と afterEach ブロックのペアを使い、それらを常に実行することで、各テストの副作用がそれ自身にとどまるようにすることです。  
仮にテストが失敗した場合でもクリーンアップコードを実行するようにするべき、ということを覚えておいてください。さもなくば、テストは「穴の開いた」ものとなってしまい、あるテストが他のテストの挙動に影響するようになってしまいます。これはデバッグを困難にします。

#### `act()`  



