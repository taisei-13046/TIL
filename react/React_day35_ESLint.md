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

4. `@typescript-eslint/ban-types`  
Bans specific types from being used  
(このルールは特定の型を禁止する)  

example
```ts
// incorrect
const str: String = 'foo';

// correct
const str: string = 'foo';
```

5. `@typescript-eslint/no-empty-interface`  
Disallow the declaration of empty interfaces  
(このルールは、意味のあるインターフェイスのみがコードで宣言されることを保証することを目的としています。)  

example
```ts
// incorrect
// an empty interface
interface Foo {}

// correct
interface Foo {
  name: string;
}
```

6. `@typescript-eslint/no-explicit-any`  
Disallow usage of the any type  
(any型を使うことは、TypeScriptを使う目的を逸脱している。anyが使われると、その値に関するコンパイラの型チェックはすべて無視される。)  

example
```ts
//incorrect 
const age: any = 'seventeen';

//correct
const age: number = 17;
```

7. `@typescript-eslint/no-extra-non-null-assertion`  
Disallow extra non-null assertion  
(余分な非NULLアサーションの禁止)  

```ts
//incorrect
const foo: { bar: number } | null = null;
const bar = foo!!!.bar;

//correct
const foo: { bar: number } | null = null;
const bar = foo!.bar;
```

8. `@typescript-eslint/no-floating-promises`  
Requires Promise-like values to be handled appropriately  
(このルールは、Promiseのような値を、そのエラーを適切に処理せずにステートメントで使用することを禁止します。)  


9. `@typescript-eslint/no-for-in-array`  
Disallow iterating over an array with a for-in loop  
(for-inループによる配列の反復処理の不許可)  
for-inループは、配列のインデックスを文字列として繰り返し処理し、配列の「穴」は無視されます。  

indexの繰り返しには以下のものが推奨されている  
```ts
array.forEach((value, index) => { ... });
for (const [index, value] of array.entries()) { ... }
for (let i = 0; i < array.length; i++) { ... }
```

10. `@typescript-eslint/no-inferrable-types`  
Disallows explicit type declarations for variables or parameters initialized to a number, string, or boolean  
(数値、文字列、ブール値で初期化された変数やパラメータに対する明示的な型宣言を禁止する。)  

```ts
//incorrect
const a: string = 'str';

//correct
const a = 'str';
```

11. `@typescript-eslint/no-misused-new`  
Enforce valid definition of new and constructor  
(newとコンストラクタの有効な定義の強制)  
クラスコンポーネントは割愛

12. `@typescript-eslint/no-misused-promises`  
Avoid using promises in places not designed to handle them  
(このルールは、TypeScriptコンパイラがプロミスを許可しているにもかかわらず、適切に処理されていない場所でのプロミスの使用を禁止するものである。このような状況は、awaitキーワードが抜けていたり、非同期関数がどのように扱われ、待たされるのかについて単に誤解していたりすることで発生することがよくあります。)  
適切なawaitが必要  

13. `@typescript-eslint/no-namespace`  
Disallow the use of custom TypeScript modules and namespaces  
(カスタムTypeScriptモジュールとネームスペースの使用を禁止する)  
カスタムTypeScriptモジュール（module foo {}）と名前空間（namespace foo {}）は、TypeScriptコードを整理するための時代遅れの方法と考えられています。ES2015のモジュール構文が好まれるようになりました(import/export)。  

14. `@typescript-eslint/no-non-null-asserted-optional-chain`
Disallows using a non-null assertion after an optional chain expression  
(オプショナル・チェーン式は、オプショナル・プロパティがヌルである場合、未定義を返すように設計されています。オプショナル連鎖式の後に NULL でないアサーションを使用するのは間違っており、コードに深刻な型安全性の穴を開けてしまいます。)  

example
```ts
//incorrect
foo?.bar!;
foo?.bar()!;

//correct
foo?.bar;
(foo?.bar).baz;
```

15. `@typescript-eslint/no-non-null-assertion`
Disallows non-null assertions using the ! postfix operator  
(ポストフィックス演算子「!」を用いた非NULLアサーションの禁止)  

```ts
// incorrect
interface Foo {
  bar?: string;
}

const foo: Foo = getFoo();
const includesBaz: boolean = foo.bar!.includes('baz');

//correct
interface Foo {
  bar?: string;
}

const foo: Foo = getFoo();
const includesBaz: boolean = foo.bar?.includes('baz') ?? false;
```

16. `@typescript-eslint/no-this-alias`
Disallow aliasing this  
(このルールでは、thisに変数を割り当てることは禁止されています。)  

17. 








