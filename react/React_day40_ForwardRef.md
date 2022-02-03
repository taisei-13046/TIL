## やったこと
ForwardRefについて

### TODO コメント
[TODOコメント](https://pleiades.io/help/webstorm/using-todo.html)  
```html
# TODO: 後でリファクタリングしたい
```

### JSDoc
```js
/**
 * スマートフォンのアクセスであるかを示す真偽値です。
 * @type {boolean}
 */
var isSmartphoneAccess = true;
```
#### 最低限必要な内容
1. 役割
2. 各引数の説明
3. 返り値の説明
4. Note なぜこれを用意したのかなどの背景や、使う上での注意点など

### JSDocコメントを読む
[JSDocコメント](https://w.atwiki.jp/aias-jsstyleguide2/pages/14.html)  

全てのファイル、クラス、メソッド、プロパティにJSDocコメントが、適切なタグとデータ型を伴って記されるべきです。また名前から明白に判断できる場合を除き、プロパティ、メソッド、メソッドの引数、メソッドの戻り値を説明する文章が含まれているべきです。  

完全文（Complete sentence）で書くことを推奨しますが、必須ではありません。完全文を使う場合は適切に大文字で開始し、句読点で終わらせましょう。  

- ブロックタグ内で改行が必要な場合、コードの改行と同様にスペース4つ分インデントする  
- JSDocも多くのHTMLタグをサポートしています。`<code>、<pre>、<tt>、<strong>、<ul>、<ol>、<li>、<a>` 等々です。  

#### メソッドと関数へのコメント
パラメータと戻り値について記述されるべきです。メソッドのパラメータと戻り値が自明である場合には、説明を省略してもかまいません。  
メソッドの説明文は三人称の平叙文で書きます。  

#### JSDocコメントのメリット
1. コードヒント/コード補完の表示
2. APIリファレンスの生成

### Typescript JSDocリファレンス
[JSDocリファレンス](https://www.typescriptlang.org/ja/docs/handbook/jsdoc-supported-types.html)  

#### `@type`
“@type”タグを使用すれば、型名(プリミティブ、TypeScript宣言やJSDocの”@typedef”タグで定義されたもの)を参照することができます。  
```ts
/**
 * @type {string}
 */
var s;
```

#### `@paramと@returns`
```ts
// パラメータは様々な構文形式で宣言することができます
/**
 * @param {string}  p1 - 文字列パラメータ
 * @param {string=} p2 - 任意のパラメータ(Closure構文)
 * @param {string} [p3] - 任意のパラメータ(JSDoc構文).
 * @param {string} [p4="test"] - デフォルト値を持つ任意のパラメータ
 * @return {string} 結果
 */
function stringsStringStrings(p1, p2, p3, p4) {
  // TODO
}
```
@paramは@typeと同じ型の構文を使用しますが、パラメータ名を追加します。 また、パラメータ名を角括弧で囲むことで、パラメータを任意のものとして宣言することもできます  











