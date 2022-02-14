## やったこと
Reactの公式を読んだ2  

その前に
### onblur(オンブラー（オンブラァー）)属性とは？
onblur属性は、要素からフォーカスが離れた際に発生するイベントの処理を設定するための属性です。

属性値にスクリプトを指定することで、要素からフォーカスが離れた際に実行される処理を設定することができます。  

## React公式
### 高階 (Higher-Order) コンポーネント

高階コンポーネント (higher-order component; HOC) はコンポーネントのロジックを再利用するための React における応用テクニックです。HOC それ自体は React の API の一部ではありません。HOC は、React のコンポジションの性質から生まれる設計パターンです。  

具体的には、**高階コンポーネントとは、あるコンポーネントを受け取って新規のコンポーネントを返すような関数です。**  

コンポーネントが props を UI に変換するのに対して、高階コンポーネントはコンポーネントを別のコンポーネントに変換します。  

#### 注意事項
**render メソッド内部で HOC を使用しないこと**  

React の差分アルゴリズム（Reconciliation と呼ばれる）は、既存のサブツリーを更新すべきかそれを破棄して新しいものをマウントすべきかを決定する際に、コンポーネントの型が同一かどうかの情報を利用します。render メソッドから返されるコンポーネントが以前の描画から返されたコンポーネントと（===で検証して）同一だった場合、React はサブツリーを新しいツリーとの差分を取りながら再帰的に更新します。コンポーネントが同一でなければ、以前のサブツリーは完全にアンマウントされます。

通常このことを考慮する必要はありません。ですが HOC に関しては考えるべきことです。  

**ref 属性は渡されない**  

高階コンポーネントの通例としては、すべての props はラップされたコンポーネントに渡されますが、ref に関してはそうではありません。これは ref 属性が（key と同様）実際のプロパティではなく、React によって特別に処理されているものだからです。HOC から出力されたコンポーネントの要素に ref 属性を追加する場合、ref 属性はラップされた内側のコンポーネントではなく、最も外側のコンテナコンポーネントを参照します。


ただ、もうほとんど使い道はなさそう。。。  
[HOC（Heigher-Order-Component）ってどうなったの？](https://qiita.com/putan/items/f38bf50422efa1a0b2cb)  


### 他のライブラリとのインテグレーション
#### DOM 操作プラグインとのインテグレーション
React は、React 以外のものが DOM に加えた変更を認識しません。React は自身の内部表現に基づいて更新内容を決定します。もし同じ DOM ノードが別のライブラリによって操作された場合、React は混乱してしまい、回復する方法がありません。

とはいえ、React と操作プラグインを組み合わせることが不可能、あるいは必ずしも難しいと言っているのではありません。それぞれがやっていることを正しく認識する必要があるのです。

コンフリクトを回避する最も簡単な方法は、React コンポーネントが更新されないようにすることです。これは、空の`<div />`のように、React から更新する理由がない要素をレンダーすることで実現できます。

#### 他のビューライブラリとのインテグレーション
React は ReactDOM.render() の柔軟性のおかげで、他のアプリケーションに組み込むことができます。

React は一般的に起動時に単一のルート React コンポーネントを DOM にロードして使用されるものですが、ReactDOM.render() はボタンのような小さなものからアプリケーション全体に至るまで、独立した UI のパーツに対して複数回呼び出すこともできます。

実際、これはまさに React が Facebook で使用されている方法でもあります。これにより React でアプリケーションを少しずつ作成し、それらを既存のサーバ側テンプレートやその他のクライアントサイドコードと組み合わせることができます。

##### render()
[render()](https://ja.reactjs.org/docs/react-dom.html#render)  
```jsx
ReactDOM.render(element, container[, callback])
```
渡された container の DOM に React 要素をレンダーし、コンポーネントへの参照（ステートレスコンポーネントの場合は null）を返します。

React 要素がすでに container にレンダーされている場合は更新を行い、最新の React 要素を反映するために必要な DOM のみを変更します。

### JSX を深く理解する
JSX とは、つまるところ React.createElement(component, props, ...children) の糖衣構文にすぎません。  

#### React 要素の型を指定する
JSX タグの先頭の部分は、React 要素の型を決定しています。

大文字で始まる型は JSX タグが React コンポーネントを参照していることを示しています。このような JSX タグはコンパイルを経てその大文字で始まる変数を直接参照するようになります。つまり JSX の `<Foo />` 式を使用する場合、Foo がスコープになければなりません。  

#### JSX 型にドット記法を使用する
JSX の中においては、ドット記法を使うことによって React コンポーネントを参照することもできます。これは単一のモジュールがたくさんの React コンポーネントをエクスポートしているような場合に便利です。  

#### ユーザ定義のコンポーネントの名前は大文字で始めること
ある要素の型が小文字から始まっているような場合、それは `<div>` や `<span>` のような組み込みのコンポーネントを参照しており、これらはそれぞれ 'div' や 'span' といった文字列に変換されて React.createElement に渡されます。一方で `<Foo />` のように大文字で始まる型は React.createElement(Foo) にコンパイルされ、JavaScript ファイルにおいて定義あるいはインポートされたコンポーネントを参照します。  

#### JSX における props
任意の JavaScript 式は {} で囲むことによって props として渡すことができます  
```jsx
<MyComponent foo={1 + 2 + 3 + 4} />
```

#### プロパティのデフォルト値は true
プロパティに値を与えない場合、デフォルトの値は true となります。そのため以下のふたつの JSX の式はまったく等しいものとなります。

```jsx
<MyTextBox autocomplete />

<MyTextBox autocomplete={true} />
```

特別な理由がある場合を除いて、このように値を省略することは推奨していません。  

ES6 におけるオブジェクトの簡略表記においては、{foo} は {foo: true} ではなく {foo: foo} を意味するため、HTML の動作に似せて作られたこの機能はかえって混乱をきたす可能性があります。


#### 属性の展開
props オブジェクトがあらかじめ存在しており、それを JSX に渡したいような場合は ... を「スプレッド」演算子として使用することで、props オブジェクトそのものを渡すことができます。そのため以下のふたつの JSX の式はまったく等しいものとなります。

```jsx
function App1() {
  return <Greeting firstName="Ben" lastName="Hector" />;
}

function App2() {
  const props = {firstName: 'Ben', lastName: 'Hector'};
  return <Greeting {...props} />;
}
```

#### 真偽値、null、undefined は無視される
true と false、null、そして undefined は子要素として渡すことができます。これらは何もレンダーしません。以下の JSX の式はすべて同じ結果となります。
```jsx
<div />

<div></div>

<div>{false}</div>

<div>{null}</div>

<div>{undefined}</div>

<div>{true}</div>
```

反対に、false、true、null、または undefined といった値を表示したいのであれば、まず文字列に変換する必要があります。  

```jsx
<div>
  My JavaScript variable is {String(myVariable)}.
</div>
```

### パフォーマンス最適化
React は UI の更新時に必要となる高コストな DOM 操作の回数を最小化するために、内部的にいくつかの賢いテクニックを使用しています。多くのアプリケーションでは React を使用するだけで、パフォーマンス向上のための特別な最適化を苦労して行わなくても、レスポンスの良いユーザインターフェースを実現できますが、それでもなお、React アプリケーションを高速化するための方法はいくつか存在します。  

### ポータル
ポータル (portal) は、親コンポーネントの DOM 階層外にある DOM ノードに対して子コンポーネントをレンダーするための公式の仕組み  

```jsx
ReactDOM.createPortal(child, container)
```

第 1 引数 (child) は React の子要素としてレンダー可能なもの、例えば、要素、文字列、フラグメントなどです。第 2 引数 (container) は DOM 要素を指定します。

通常、コンポーネントの render メソッドから要素を返すと、最も近い親ノードの子として DOM にマウントされます。

```jsx
render() {
  // React は新しい div 要素をマウントし、子をその中に描画します
  return (
    <div>
      {this.props.children}
    </div>
  );
}
```
しかし、時に子要素を DOM 上の異なる位置に挿入したほうが便利なことがあります。

```jsx
render() {
  // React は新しい div をつくり*ません*。子要素は `domNode` に対して描画されます。
  // `domNode` は DOM ノードであれば何でも良く、 DOM 構造内のどこにあるかは問いません。
  return ReactDOM.createPortal(
    this.props.children,
    domNode
  );
}
```

ポータルの典型的なユースケースとは、親要素が overflow: hidden や z-index のスタイルを持っていても、子要素がコンテナを「飛び出して」見える必要があるものです。例えば、ダイアログ、ホバーカード、ツールチップがそれに当たります。  


参考資料
- [入門者でもわかるReactのPortalsの設定方法](https://reffect.co.jp/react/react-portals)  
- [React Portalsの活用（Modal作り）](https://zenn.dev/maktub_bros/articles/a700d189c60ca8)

### プロファイラ API
Profiler を使って、React アプリケーションのレンダーの頻度やレンダーの「コスト」を計測することができます。 本機能の目的は、アプリケーション中の、低速でメモ化などの最適化が有効な可能性のある部位を特定する手助けをすることです。  

#### 使用法
Profiler は React ツリー内の特定部位におけるレンダーのコストを計測するため、ツリー内のどこにでも追加できます。 2 つの props が必要です。id（文字列）と、ツリー内のコンポーネントが更新を「コミット」した際に React が毎回呼び出す onRender コールバック（関数）です。

```jsx
render(
  <App>
    <Profiler id="Navigation" onRender={callback}>
      <Navigation {...props} />
    </Profiler>
    <Main {...props} />
  </App>
);
```

#### onRender コールバック
Profiler には props として onRender 関数を渡す必要があります。 プロファイリングされているツリー内のコンポーネントが更新を「コミット」した際に、React がこの関数を毎回呼び出します。 この関数は、レンダー内容とかかった時間に関する情報を引数として受け取ります。  

```jsx
function onRenderCallback(
  id, // the "id" prop of the Profiler tree that has just committed
  phase, // either "mount" (if the tree just mounted) or "update" (if it re-rendered)
  actualDuration, // time spent rendering the committed update
  baseDuration, // estimated time to render the entire subtree without memoization
  startTime, // when React began rendering this update
  commitTime, // when React committed this update
  interactions // the Set of interactions belonging to this update
) {
  // Aggregate or log render timings...
}
```

参考資料
- [React Profilerの活用](https://zenn.dev/maktub_bros/articles/814a14312d2393)  

### 差分検出処理
React は、各更新で実際に何が変更されるべきかを人間が心配する必要がないように、宣言型の API を提供しています。これによりアプリケーションの作成が大幅に容易になるわけですが、React の中でこの処理がどのように実装されているのかはよく分からないかもしれません。  

React を使う際、render() 関数をある時点の React 要素のツリーを作成するものとして考えることができます。次回の state や props の更新時には、render() 関数は React 要素の別のツリーを返します。React はそこから直近のツリーに合致させるように効率よく UI を更新する方法を見つけ出す必要があります。

あるツリーを別のものに変換するための最小限の操作を求めるというアルゴリズム問題については、いくつかの一般的な解決方法が存在しています。しかし、最新のアルゴリズムでもツリーの要素数を n として O(n3) ほどの計算量があります。  

React でそのアルゴリズムを使った場合、1000 個の要素を表示するのに 10 億といったレベルの比較が必要となります。これではあまりに計算コストが高すぎます。代わりに、React は 2 つの仮定に基づくことで、ある程度近い結果を得ることができる O(n) ほどの計算量のアルゴリズムを実装しています。  

1. 異なる型の 2 つの要素は異なるツリーを生成する。
2. 開発者は key プロパティを与えることで、異なるレンダー間でどの子要素が変化しない可能性があるのかについてヒントを出すことができる。

#### 差分アルゴリズム
**異なる型の要素**  
ルート要素が異なる型を持つ場合は常に、React は古いツリーを破棄して新しいツリーをゼロから構築します。`<a>` から `<img>` へ、もしくは `<Article>` から `<Comment>` へ、もしくは `<Button>` から `<div>` へ ― それらの全てがツリーをゼロから再構築させるのです。

**子要素の再帰的な処理**  
デフォルトでは、DOM ノードの子に対して再帰的に処理を行う場合、React は単純に、両方の子要素リストのそれぞれ最初から同時に処理を行っていって、差分を見つけたところで毎回更新を発生させます。  

例えば、子ノードの最後にひとつ要素を追加するような場合、以下の 2 つのツリー間の変換はうまく動作します：  

```jsx
<ul>
  <li>first</li>
  <li>second</li>
</ul>

<ul>
  <li>first</li>
  <li>second</li>
  <li>third</li>
</ul>
```
React は 2 つの `<li>first</li>` ツリーを一致させ、2 つの `<li>second</li>` ツリーを一致させ、最後に `<li>third</li>` ツリーを挿入します。

それを単純に実行した場合、先頭への要素の追加はパフォーマンスが悪くなります。

```jsx
<ul>
  <li>Duke</li>
  <li>Villanova</li>
</ul>

<ul>
  <li>Connecticut</li>
  <li>Duke</li>
  <li>Villanova</li>
</ul>
```

React は `<li>Duke</li>` と `<li>Villanova</li>` サブツリーをそのまま保てるということに気づくことなく、すべての子要素を変更してしまいます。この非効率性は問題になることがあります。

**Keys**  
この問題を解決するため、React は key 属性をサポートします。子要素が key を持っている場合、React は key を利用して元のツリーの子要素と次のツリーの子要素を対応させます。例えば、key を前出の非効率な例に追加することで、ツリーの変換を効率的なものにすることができます。

### Ref と DOM
Ref は render メソッドで作成された DOM ノードもしくは React の要素にアクセスする方法を提供します。  

**いつ Ref を使うか**
Ref に適した使用例は以下の通りです。

- フォーカス、テキストの選択およびメディアの再生の管理
- アニメーションの発火
- サードパーティの DOM ライブラリとの統合

**Ref を使いすぎない**  

最初はアプリ内で「何かを起こす」ために ref を使いがちかもしれません。そんなときは、少し時間をかけて、コンポーネントの階層のどこで状態を保持すべきかについて、よりしっかりと考えてみてください。多くの場合、その状態を「保持する」ための適切な場所は階層のより上位にあることが明らかになるでしょう。  

### strict モード

StrictMode はアプリケーションの潜在的な問題点を洗い出すためのツールです。Fragment と同様に、StrictMode は目に見える UI を描画しません。StrictMode の子孫要素に対しては、付加的な検査および警告が動くようになります。  


















