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


### ForwardRefについて
ref のフォワーディングはあるコンポーネントを通じてその子コンポーネントのひとつに ref を自動的に渡すテクニックです。  
まずはRef と DOMについておさらい  
[Ref と DOM](https://ja.reactjs.org/docs/refs-and-the-dom.html)  
**Ref は render メソッドで作成された DOM ノードもしくは React の要素にアクセスする方法を提供します。**  

#### いつ Ref を使うか
- フォーカス、テキストの選択およびメディアの再生の管理
- アニメーションの発火
- サードパーティの DOM ライブラリとの統合

#### Ref を使いすぎない
最初はアプリ内で「何かを起こす」ために ref を使いがちかもしれません。そんなときは、少し時間をかけて、コンポーネントの階層のどこで状態を保持すべきかについて、よりしっかりと考えてみてください。多くの場合、その状態を「保持する」ための適切な場所は階層のより上位にあることが明らかになるでしょう。  
state のリフトアップガイドを参照  
[state のリフトアップ](https://ja.reactjs.org/docs/lifting-state-up.html)  

#### state のリフトアップ
**しばしば、いくつかのコンポーネントが同一の変化するデータを反映する必要がある場合があります。そんなときは最も近い共通の祖先コンポーネントへ共有されている state をリフトアップすることを推奨します。**  
React アプリケーションで変化するどのようなデータも単一の “信頼出来る情報源” であるべきです。通常、state はレンダー時にそれを必要とするコンポーネントに最初に追加されます。それから、他のコンポーネントもその state を必要としているなら、直近の共通祖先コンポーネントにその state をリフトアップすることができます。  

#### Ref を作成する
Ref は React.createRef() を使用して作成され、ref 属性を用いて React 要素に紐付けられます。  
Ref は通常、コンポーネントの構築時にインスタンスプロパティに割り当てられるため、コンポーネントを通して参照が可能です。  

#### Ref へのアクセス
ref が render メソッドの要素に渡されると、そのノードへの参照は ref の current 属性でアクセスできるようになります。

```ts
const node = this.myRef.current;
```

ref の値はノードの種類によって異なります。  
- HTML 要素に対して ref 属性が使用されている場合、React.createRef() を使ってコンストラクタ内で作成された ref は、その current プロパティとして根底にある DOM 要素を受け取ります 
  -  コンポーネントがマウントされると React は current プロパティに DOM 要素を割り当て、マウントが解除されると null に戻します。
- ref 属性がカスタムクラスコンポーネントで使用されるとき、ref オブジェクトはコンポーネントのマウントされたインスタンスを current として受け取ります
- 関数コンポーネント (function components) には ref 属性を使用してはいけません。なぜなら、関数コンポーネントはインスタンスを持たないからです
  - 関数コンポーネントに対して ref が使用できるようにしたい場合は、forwardRef を（必要に応じて useImperativeHandle と組み合わせて）利用するか、コンポーネントをクラスに書き換えます。

#### DOM の Ref を親コンポーネントに公開する
Ref のフォワーディングを使うと、コンポーネントは任意の子コンポーネントの ref を自分自身の ref として公開できるようになります。  

#### コールバック Ref
createRef() によって作成された ref 属性を渡す代わりに、関数を渡します。この関数は、引数として React コンポーネントのインスタンスまたは HTML DOM 要素を受け取ります。これを保持することで、他の場所からアクセスできます。  

#### DOM コンポーネントに ref をフォワーディングする
(例)ネイティブの button DOM 要素をレンダーする FancyButton というコンポーネント  
``tsx
function FancyButton(props) {
  return (
    <button className="FancyButton">
      {props.children}
    </button>
  );
}
```










