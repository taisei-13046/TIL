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
dangerouslySetInnerHTML は、ブラウザ DOM における innerHTML の React での代替です。一般に、コードから HTML を設定することは、誤ってあなたのユーザをクロスサイトスクリプティング (XSS) 攻撃に晒してしまいやすいため、危険です。そのため、React では直接 HTML を設定することはできますが、それは危険であることを自覚するために dangerouslySetInnerHTML と入力し __html というキーを持つオブジェクトを渡す必要があります。  

**style**  

style 属性は CSS 文字列ではなく、キャメルケースのプロパティを持った JavaScript オブジェクトを受け取ります。これは JavaScript での DOM の style プロパティとの一貫性があり、より効率的で、XSS 攻撃の対象となるセキュリティホールを防ぎます。  

React はインラインスタイルでの特定の数値プロパティに対して自動的に “px” サフィックスを付け加えます。“px” 以外の単位を使用したい場合は、その単位を付け加えた文字列で値を指定してください。  

```jsx
// 最終的なスタイルは '10px'
<div style={{ height: 10 }}>
  Hello World!
</div>

// 最終的なスタイルは '10%'
<div style={{ height: '10%' }}>
  Hello World!
</div>
```


### 合成イベント (SyntheticEvent)
イベントハンドラには、SyntheticEvent のインスタンスが渡されます。これはブラウザのネイティブイベントに対するクロスブラウザ版のラッパです。stopPropagation() と preventDefault() を含む、ブラウザのネイティブイベントと同じインターフェイスを持ちつつ、ブラウザ間で同じ挙動をするようになっています。  

何らかの理由で実際のブラウザイベントが必要な場合は、単に nativeEvent 属性を使用するだけで取得できます。合成イベントはブラウザのネイティブイベントとは別物であり、直接の対応があるわけでもありません。例えば onMouseLeave イベントの場合、event.nativeEvent は mouseout イベントになっています。個々の対応については公開 API の範疇ではなく、常に変わる可能性があります。すべての SyntheticEvent オブジェクトは以下の属性を持っています。  

#### フォーカスイベント
```
onFocus onBlur
```
**フォーカスが当たった・外れたことの検出**  
currentTarget と relatedTarget を用いることで、フォーカスが当たった・外れた際のイベントが親要素の外側で起こったかどうかを判定できます。以下のコピー・ペーストで使えるデモでは、子要素のどれかへのフォーカス、要素自身へのフォーカス、サブツリー全体から出入りするフォーカスを、それぞれどのように検出するかを示しています。

### テストユーティリティ
#### act()
アサーション用のコンポーネントを準備するために、それをレンダーして更新を実行するコードを act() でラップします。これにより、テストはブラウザでの React の動作により近い状態で実行されます。  

#### isElement()
element が任意の React 要素である場合 true を返します。

#### isElementOfType()
element が componentClass 型の React 要素である場合 true を返します。

#### isDOMComponent()
instance が DOM コンポーネント（`<div>` や `<span>` など）である場合 true を返します。

などなど

テストに関しては別日にまとめて勉強しよう  

### React 用語集
#### シングルページアプリケーション

シングルページアプリケーション (single-page application) は、単一の HTML ページでアプリケーションの実行に必要なすべてのアセット（JavaScript や CSS など）をロードするようなアプリケーションです。初回ページもしくはそれ以降のページでのユーザとのやりとりにおいて、サーバとの往復が不要、すなわちページのリロードが発生しません。  

React でシングルページアプリケーションを構築することができますが、そうすることは必須ではありません。React は、既存のウェブサイトの小さな一部分を拡張してよりインタラクティブにするために使用することもできます。React で記述されたコードは、PHP などによりサーバでレンダリングされたマークアップや、他のクライアントサイドのライブラリと問題なく共存できます。実際に、Facebook では React はそのように利用されています。  

#### コンパイラ
JavaScript コンパイラは JavaScript コードを受け取って変換し、別のフォーマットの JavaScript コードを返します。最も一般的なユースケースは ES6 構文を受け取り、古いブラウザーが解釈できる構文に変換することです。Babel は React とともに利用されることが最も多いコンパイラです。  

#### バンドラ

バンドラは別々のモジュールとして記述された（しばしば数百個になる）JavaScript および CSS のコードを受け取り、ブラウザに最適化された数個のファイルに結合します。Webpack や Browserify を含む、いくつかのバンドラが React アプリケーションで一般的に利用されています。  

#### パッケージマネージャ
パッケージ マネージャーは、プロジェクト内の依存関係を管理するためのツールです。npm および Yarn の 2 つのパッケージマネージャが React アプリケーションで一般的に利用されています。どちらも同じ npm パッケージレジストリのクライアントです。  

#### CDN
CDN は Content Delivery Network の略です。CDN はキャッシュされた静的なコンテンツをネットワーク化されたサーバから世界中に配信します。  

#### 要素
React 要素は React アプリケーションを構成するブロックです。要素を、より広く知られている概念である “コンポーネント” と混同する人もいるかもしれません。要素はあなたが画面上に表示したいものの説明書きとなるものです。React 要素はイミュータブルです。

```jsx
const element = <h1>Hello, world</h1>;
```

通常、要素は直接使用されるものではなく、コンポーネントから返されるものです。

#### コンポーネント
React のコンポーネントとは、ページに表示される React 要素を返す、小さく再利用可能なコードのことです。もっともシンプルな形の React コンポーネントは、React 要素を返すプレーンな JavaScript 関数です  

#### 制御されたコンポーネント vs. 非制御コンポーネント
React では、フォームの入力を扱うのに 2 つの異なるアプローチがあります。

React によって値が制御される入力フォーム要素は制御されたコンポーネント (controlled component) と呼ばれます。ユーザが制御されたコンポーネントにデータを入力すると、変更イベントハンドラがトリガーされ、コードが入力が有効であるか（更新された値で再レンダーすることで）決定します。再レンダーしなければフォーム要素は変更されないままとなります。

非制御コンポーネント (uncontrolled component) は React の管理外にあるフォーム要素と同様に動作します。ユーザがフォームフィールド（入力ボックス、ドロップダウンなど）にデータを入力した場合、更新された情報が反映され、その際に React は何もする必要がありません。しかし、このことはフィールドに特定の値を設定できないということでもあります。

ほとんどの場合では、制御されたコンポーネントを使用するべきでしょう。

#### リコンシリエーション（更新検出処理）
コンポーネントの props か state が変更された場合、React は新しく返された要素を以前にレンダーされた要素と比較することで、本物の DOM の更新が必要かを判断します。それらが等しくない場合、React は DOM を更新します。このプロセスを “リコンシリエーション (reconciliation)” と呼びます。  
