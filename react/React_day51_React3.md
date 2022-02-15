## やったこと
Reactの公式を読む3

## レビューまとめ
### `{...rest}`を渡す対象はTopレベルの要素に！
❌ incorrect
```tsx
export const Component = ({ isOpen, ...rest }: ComponentProps) => {
  return (
    <Container>
      <CompWrapper isOpen={isOpen} {...rest}>
```
⭕️ correct
```tsx
export const Component = ({ isOpen, ...rest }: ComponentProps) => {
  return (
    <Container {...rest}>
      <CompWrapper isOpen={isOpen}>
```

Topレベルに{...rest}を渡さないと、className経由でcssを当てる際に意図しない結果になる  

### cssの`gap`について
今の今までgapの存在を知らなかった。。。  
[gap](https://developer.mozilla.org/ja/docs/Web/CSS/gap)  

gap は CSS のプロパティで、行や列の間のすき間 (溝) を定義します。これは row-gap および column-gap の一括指定です。  



## React公式
### React の最上位 API

React は React ライブラリのエントリポイントです。`<script>` タグから React を読み込む場合、これらの最上位 API をグローバルの React から利用できます。npm と ES6 を使う場合、import React from 'react' と書けます。  

#### React.Component
[React.Component](https://ja.reactjs.org/docs/react-component.html)  

React コンポーネントが ES6 クラス を用いて定義されている場合、React.Component はそれらの基底クラスになります。  

#### React.memo
React.memo は高階コンポーネントです。

もしあるコンポーネントが同じ props を与えられたときに同じ結果をレンダーするなら、結果を記憶してパフォーマンスを向上させるためにそれを React.memo でラップすることができます。つまり、React はコンポーネントのレンダーをスキップし、最後のレンダー結果を再利用します。  

React.memo は props の変更のみをチェックします。React.memo でラップしているあなたのコンポーネントがその実装内で useState、useReducer や useContext フックを使っている場合、state やコンテクストの変化に応じた再レンダーは発生します。  

デフォルトでは props オブジェクト内の複雑なオブジェクトは浅い比較のみが行われます。比較を制御したい場合は 2 番目の引数でカスタム比較関数を指定できます。  

```jsx
function MyComponent(props) {
  /* render using props */
}
function areEqual(prevProps, nextProps) {
  /*
  nextProps を render に渡した結果が
  prevProps を render に渡した結果となるときに true を返し
  それ以外のときに false を返す
  */
}
export default React.memo(MyComponent, areEqual);
```

これはパフォーマンス最適化のためだけの方法です。バグを引き起こす可能性があるため、レンダーを「抑止する」ために使用しないでください。

#### isValidElement()
```jsx
React.isValidElement(object)
```

オブジェクトが React 要素であることを確認します。true または false を返します。

#### React.Children
React.Children はデータ構造が非公開の this.props.children を扱うためのユーティリティを提供します。

**React.Children.map**  
```jsx
React.Children.map(children, function[(thisArg)])
```

this を thisArg に設定して、children 内に含まれるすべての直下の子要素に対して関数を呼び出します。children が配列の場合は走査され、配列の各要素に対して関数が呼び出されます。children が null または undefined の場合はこのメソッドは配列ではなく null または undefined を返します。  

**React.Children.count**  
```jsx
React.Children.count(children)
```

children に含まれるコンポーネントの総数を返します。これは map または forEach に渡したコールバックが呼ばれる回数と同じです。

**React.Children.only**  

```jsx
React.Children.only(children)
```

children が 1 つの子要素しか持たないことを確認し、結果を返します。そうでない場合、このメソッドはエラーを投げます。

**React.Children.toArray**  
```jsx
React.Children.toArray(children)
```

データ構造が非公開の children を平坦な配列として返し、それぞれの要素に key を割り当てます。レンダーメソッド内で子の集合を操作したい場合、特に this.props.children を渡す前に並べ替えたりスライスしたい場合に便利です。  

#### React.createRef
React.createRef は ref を作成します。ref は ref 属性を介して React 要素に取り付けることができます。  

#### React.forwardRef
React.forwardRef は ref を配下のツリーの別のコンポーネントに受け渡す React コンポーネントを作成します。  
React.forwardRef はレンダー関数を引数として受け入れます。React は props と ref を 2 つの引数として呼び出します。この関数は React ノードを返す必要があります。  

#### React.lazy
React.lazy() を使用すると、動的に読み込まれるコンポーネントを定義できます。これにより、バンドルサイズを削減して、最初のレンダー時に使用されないコンポーネントの読み込みを遅らせることができます。  

```jsx
// This component is loaded dynamically
const SomeComponent = React.lazy(() => import('./SomeComponent'));
```

lazy コンポーネントをレンダーするには <React.Suspense> がレンダリングツリーの上位に必要です。これはローディングインジケータを指定する方法です。

#### React.Suspense
React.Suspense を使用することで、その配下にレンダーする準備ができていないコンポーネントがあるときにローディングインジケータを指定できます。現在、遅延読み込みコンポーネントは `<React.Suspense>` のみによってサポートされています。

遅延される (lazy) コンポーネントを Suspense ツリーの奥深くに置くことができ、それらを 1 つずつラップする必要はありません。ベストプラクティスは `<Suspense>` をローディングインジケータを表示したい場所に配置することですが、コードを分割したい場合は lazy() を使用してください。

### ReactDOM

`<script>` タグから React をロードすると、以下のトップレベル API をグローバル変数 ReactDOM で使用することができます。npm と ES6 を使用している場合は、import ReactDOM from 'react-dom' と記述できます。  

#### render()
```jsx
ReactDOM.render(element, container[, callback])
```

渡された container の DOM に React 要素をレンダーし、コンポーネントへの参照（ステートレスコンポーネントの場合は null）を返します。

React 要素がすでに container にレンダーされている場合は更新を行い、最新の React 要素を反映するために必要な DOM のみを変更します。

オプションのコールバックが渡されている場合は、コンポーネントがレンダーまたは更新された後に実行されます。

#### hydrate()
```jsx
ReactDOM.hydrate(element, container[, callback])
```

render() と同様ですが、ReactDOMServer により HTML コンテンツが描画されたコンテナをクライアントで再利用するために使用されます。React は既存のマークアップにイベントリスナをアタッチしようとします。  

React はレンダーされる内容が、サーバ・クライアント間で同一であることを期待します。React はテキストコンテンツの差異を修復することは可能ですが、その不一致はバグとして扱い、修正すべきです。開発用モードでは、React は両者のレンダーの不一致について警告します。不一致がある場合に属性の差異が修復されるという保証はありません。これはパフォーマンス上の理由から重要です。なぜなら、ほとんどのアプリケーションにおいて不一致が発生するということは稀であり、全てのマークアップを検証することは許容不可能なほど高コストになるためです。  

単一要素の属性やテキストコンテンツがサーバ・クライアント間においてやむを得ず異なってしまう場合（例えばタイムスタンプなど）、要素に suppressHydrationWarning={true} を追加することで警告の発生を停止させることが可能です。それは 1 階層下の要素までで機能するものであり、また避難ハッチとして使われるものです。そのため、多用しないでください。テキストコンテンツでない限り、React は修復を試行しようとはしないため、将来の更新まで不整合が残る可能性があります。  


これは割とnext.jsを使うときに重要になってくるのかな？  

#### unmountComponentAtNode()
```jsx
ReactDOM.unmountComponentAtNode(container)
```
DOM からマウントされた React コンポーネントを削除し、イベントハンドラや state をクリーンアップします。コンテナにコンポーネントがマウントされていない場合、このメソッドを呼び出しても何も行いません。コンポーネントがアンマウントされた場合は true を返し、アンマウントすべきコンポーネントが存在しなかった場合は false を返します。  

#### findDOMNode()
```jsx
ReactDOM.findDOMNode(component)
```

DOM にこのコンポーネントがマウントされている場合、このメソッドは対応するネイティブブラウザの DOM 要素を返します。このメソッドはフォームフィールドの値や DOM の大きさを計測するのに便利です。ほとんどのケースにおいて、DOM ノードに ref をアタッチすることで findDOMNode の使用を避けることができます。  

### ReactDOMServer
ReactDOMServer オブジェクトはコンポーネントを静的なマークアップとして変換できるようにします。これは、一般的に Node サーバで使われます。  

#### renderToString()

React 要素を初期状態の HTML へと変換します。React は HTML 文字列を返します。このメソッドにより、サーバ上で HTML を生成して最初のリクエストに対してマークアップを送信してページ読み込み速度を向上させたり、また SEO 目的で検索エンジンがページを巡回することを可能にします。  

#### renderToStaticMarkup()
React が内部的に使用する data-reactroot のような追加の DOM 属性を作成しないことを除いて、renderToString と同様の動作をします。このメソッドは React を単純な静的サイトジェネレータとして使用したい場合に便利で、追加の属性を省略することでバイト数を削減できます。  

#### renderToNodeStream()
React 要素を初期状態の HTML へと変換します。HTML の文字列を出力する Readable ストリームを返します。このストリームによる HTML 出力は ReactDOMServer.renderToString が返すものと全く同じです。このメソッドにより、サーバ上で HTML を生成して最初のリクエストに対してマークアップを送信してページ読み込み速度を向上させたり、また SEO 目的で検索エンジンがページを巡回することを可能にします。  

#### renderToStaticNodeStream()
React が内部的に使用する data-reactroot のような追加の DOM 属性を作成しないことを除いて、renderToNodeStream と同様の動作をします。このメソッドは React を単純な静的サイトジェネレータとして使用したい場合に便利で、追加の属性を省略することでバイト数を削減できます。  

### DOM 要素
React はパフォーマンスとブラウザ間での互換性のために、ブラウザから独立した DOM システムを実装しています。このことを機に、ブラウザの DOM 実装にあるいくつかの粗削りな部分が取り払われました。  

React では、DOM のプロパティと属性（イベントハンドラを含む）全てがキャメルケースで名前付けされる必要があります。例えば、HTML 属性 tabindex に React で対応する属性は tabIndex です。例外は aria-* と data-* 属性であり、これらは全て小文字に揃える必要があります。例えば、aria-label は aria-label のままにできます。


#### 属性についての差異
**checked**  

checked 属性はインプットタイプが checkbox または radio の `<input>` コンポーネントでサポートされています。コンポーネントがチェックされた状態かどうかの設定に、この属性を使うことができます。これは制御されたコンポーネント (controlled component) を構築する際に役立ちます。defaultChecked は非制御コンポーネント (uncontrolled component) において同様の働きをする属性で、そのコンポーネントが最初にマウントされた時に、チェックされた状態かどうかを設定します。

**className**   
CSS クラスを指定するには、className 属性を使用してください。このことは `<div>、<a>` など全ての標準 DOM 要素と SVG 要素に当てはまります。

React を（一般的ではありませんが）Web Components とともに使用する場合は、代わりに class 属性を使用してください。

**dangerouslySetInnerHTML**  




















