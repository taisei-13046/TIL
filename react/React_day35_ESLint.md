## やったこと

ESLintのrecommendについて調べる

### @typescript-eslint/recommendedって何をチェックしてるの？
[typescript-eslint/typescript-eslint](https://github.com/typescript-eslint/typescript-eslint/tree/main/packages/eslint-plugin)  

1. `@typescript-eslint/adjacent-overload-signatures`  
Require that member overloads be consecutive  
(この規則は、オーバーロードされたメンバの編成方法を標準化することを目的としている。)  

2. `@typescript-eslint/await-thenable`  
Disallows awaiting a value that is not a Thenable  
(このルールは、"Thenable"（Promiseのようにthenメソッドを持つオブジェクト）ではない値を待つことを禁止しています。Promiseでない値を待つことはJavaScriptとして有効ですが（すぐに解決します）、このパターンはプログラマのミスであることが多く、例えばPromiseを返す関数を呼び出す際に括弧を付け忘れたりすることがあります。)  

incorrect
```ts
await 'value';
```

correct
```ts
await Promise.resolve('value');
```

3. `@typescript-eslint/ban-ts-comment`  
Bans `@ts-<directive>` comments from being used or requires descriptions after directive  
(TypeScriptには、ファイルを処理する方法を変更するために使用できる、いくつかの指示コメントがあります。TypeScript Compiler Errorsを抑制するためにこれらを使用すると、TypeScript全体の効果が減少してしまう。)  
example
```ts
// @ts-expect-error
// @ts-ignore
// @ts-nocheck
// @ts-check
```
参考記事 [TypeScriptの型エラーを踏み潰すときのお作法](https://qiita.com/uhyo/items/a354d4135e3dec15d01e#%E8%89%B2%E3%80%85%E3%81%AA%E3%82%A8%E3%83%A9%E3%83%BC%E3%81%AE%E6%B6%88%E3%81%97%E6%96%B9)  





