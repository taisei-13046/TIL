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






