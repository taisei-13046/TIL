## やったこと
Reactの公式を復習した  
Main Conceptsは大丈夫！ Advanced guideを見ていく  
[advanced guide](https://ja.reactjs.org/docs/accessibility.html)  

## 案件内の発見
### typescriptの型の拡張
**中継型を経由すれば実現可能**  

```ts
interface A {
  foo: number
  bar: number
  baz: number
  qux: number
  yo: number
}

// weakな中継用のinterfaceを用意する
interface Bweaken extends A {
  yo: any
}

interface B extends Bweaken {
  cha: string
  yo: string
}
```
このように一度any型にしてから上書きすることはできる。  
でもすごい違和感を感じる...

```ts
type Weaken<T, K extends keyof T> = {
  [P in keyof T]: P extends K ? any : T[P];
};
```
一応こんなutilsを提案してる人もいた  
[(suggestion) Overrides in extending class and interface definitions. #3402](https://github.com/Microsoft/TypeScript/issues/3402)  



## React公式
### アクセシビリティ
Web アクセシビリティ（a11y とも呼ばれます）とは、誰にでも使えるようウェブサイトを設計・構築することです。ユーザ補助技術がウェブページを解釈できるようにするためには、サイトでアクセシビリティをサポートする必要があります。  

#### セマンティックな HTML
セマンティック（意味論的）な HTML はウェブアプリケーションにおけるアクセシビリティの基礎となります。ウェブサイト内の情報の意味を明確にするための多様な HTML 要素を使うことにより、大抵の場合は少ない手間でアクセシビリティを手に入れられます。  

ときおり、React コードを動くようにするために JSX に `<div>` を追加すると、HTML のセマンティックが崩れることがあります。とりわけ、リスト (`<ol>, <ul>, <dl>`) や `<table>` タグと組み合わせるときに問題になります。そんなときは複数の要素をグループ化するために React フラグメントを使う方がよいでしょう。  

```jsx
function ListItem({ item }) {
  return (
    <>
      <dt>{item.term}</dt>
      <dd>{item.description}</dd>
    </>
  );
}
```

#### アクセシブルなフォーム
`<input> や <textarea>` のような各 HTML フォームコントロールには、アクセシブルな形でのラベル付けが必要です。スクリーンリーダに公開される、説明的なラベルを提供する必要があります。  
React でこれらの標準的な HTML の実践知識を直接使用できますが、JSX では for 属性は htmlFor として記述されることに注意してください。  

```jsx
<label htmlFor="namedInput">Name:</label>
<input id="namedInput" type="text" name="name"/>
```

#### フォーカス制御
**キーボードフォーカスとフォーカス時のアウトライン（輪郭線）**  

React アプリケーションは実行されている間、継続的に HTML の DOM を変更するため、時にキーボードフォーカスが失われたり、予期しない要素にセットされたりすることがあります。これを修正するためには、プログラムによってキーボードフォーカスを正しい位置に移動させる必要があります。例えばモーダルウィンドウを閉じた後には、モーダルを開いたボタンにキーボードフォーカスを戻すことなどです。  

コンポーネントを拡張するのに高階コンポーネント (HOC) を使う場合は、React の forwardRef 関数を用いて、関数に囲われたコンポーネントに ref をフォワーディング (forwarding) する ことをおすすめします。もし、サードパーティの高階コンポーネントが ref フォワーディングを実装していないときでも、上記のパターンはフォールバックとして使えます。  

#### マウスとポインタのイベント
マウスまたは、ポインタのイベントを通じて使われる機能がキーボード単体でも同じように使用できるようにしてください。ポインタデバイスだけに依存した実装は、多くの場合にキーボードユーザがアプリケーションを使えない原因になります。

`onClick`で可能なイベントだけでは、キーボード操作のみの作業はできない！  
そのため、**onBlur** と **onFocus** のような適切なイベントハンドラを代わりに用いる必要がある  

#### より複雑なウィジェット
ユーザ体験がより複雑であるほど、よりアクセシビリティが損なわれるということがあってはいけません。できるだけ HTML に近くなるようコーディングすればアクセシビリティを最も簡単に達成できますが、一方でかなり複雑なウィジェットでもアクセシビリティを保ってコーディングすることができます。  

#### 開発とテストのツール
**キーボード**  
最も簡単で最も重要なチェックのうちのひとつは、ウェブサイト全体がキーボード単体であまねく探索でき、使えるかどうかのテストです。これは以下の手順でチェックできます。

1. マウスを外します。
2. Tab と Shift+Tab を使ってブラウズします。
3. 要素を起動するのに Enter を使用します。
4. 必要に応じて、キーボードの矢印キーを使ってメニューやドロップダウンリストなどの要素を操作します。

### コード分割
#### バンドル
多くの React アプリケーションは、Webpack、Rollup や Browserify などのツールを使ってファイルを「バンドル」しています。バンドルはインポートされたファイルをたどって、それらを 1 つのファイルにまとめるプロセスです。このバンドルされたファイルを Web ページ内に置くことによって、アプリ全体を一度に読み込むことができます。

バンドルは確かに素晴らしいですが、アプリが大きくなるにつれて、バンドルのサイズも大きくなります。特にサイズの大きなサードパーティ製のライブラリを含む場合は顕著にサイズが増大します。不用意に大きなバンドルを作成してしまいアプリの読み込みに多くの時間がかかってしまうという事態にならないためにも、常に注意を払い続けなければなりません。  

大きなバンドルを不注意に生成してしまわないように、あらかじめコードを「分割」して問題を回避しましょう。 Code-Splitting は、Webpack、Rollup や Browserify（factor-bundle を使用）などのバンドラによってサポートされている機能であり、実行時に動的にロードされる複数のバンドルを生成することができます。

コード分割は、ユーザが必要とするコードだけを「**遅延読み込み**」する手助けとなり、アプリのパフォーマンスを劇的に向上させることができます。アプリの全体的なコード量を減らすことはできませんが、ユーザが必要としないコードを読み込まなくて済むため、初期ロードの際に読む込むコード量を削減できます。

### コンテクスト
**コンテクストをいつ使用すべきか?**   
コンテクストは、ある React コンポーネントのツリーに対して「グローバル」とみなすことができる、現在の認証済みユーザ・テーマ・優先言語といったデータを共有するために設計されています。  
コンテクストを使用することで、中間の要素群を経由してプロパティを渡すことを避けることができます。  

**コンテクストを使用する前に**  
コンテクストは主に、何らかのデータが、ネストレベルの異なる多くのコンポーネントからアクセスできる必要がある時に使用されます。コンテクストはコンポーネントの再利用をより難しくする為、慎重に利用してください。

もし多くの階層を経由していくつかの props を渡すことを避けたいだけであれば、コンポーネントコンポジションは多くの場合、コンテクストよりシンプルな解決策です。  

例えば、深くネストされた Link と Avatar コンポーネントがプロパティを読み取ることが出来るように、user と avatarSize プロパティをいくつかの階層下へ渡す Page コンポーネントを考えてみましょう。  

```jsx
<Page user={user} avatarSize={avatarSize} />
// ... Page コンポーネントは以下をレンダー ...
<PageLayout user={user} avatarSize={avatarSize} />
// ... PageLayout コンポーネントは以下をレンダー ...
<NavigationBar user={user} avatarSize={avatarSize} />
// ... NavigationBar コンポーネントは以下をレンダー ...
<Link href={user.permalink}>
  <Avatar user={user} size={avatarSize} />
</Link>
```
コンテクストを使用せずにこの問題を解決する 1 つの手法は、Avatar コンポーネント自身を渡すようにするというもので、そうすれば間のコンポーネントは user や avatarSize プロパティを知る必要はありません  

```jsx
function Page(props) {
  const user = props.user;
  const userLink = (
    <Link href={user.permalink}>
      <Avatar user={user} size={props.avatarSize} />
    </Link>
  );
  return <PageLayout userLink={userLink} />;
}

// これで以下のようになります。
<Page user={user} avatarSize={avatarSize} />
// ... Page コンポーネントは以下をレンダー ...
<PageLayout userLink={...} />
// ... PageLayout コンポーネントは以下をレンダー ...
<NavigationBar userLink={...} />
// ... NavigationBar コンポーネントは以下をレンダー ...
{props.userLink}
```

この制御の反転はアプリケーション内で取り回す必要のあるプロパティの量を減らし、ルートコンポーネントにより多くの制御を与えることにより、多くのケースでコードを綺麗にすることができます。しかし、このような制御の反転がすべてのケースで正しい選択となるわけではありません。ツリー内の上層に複雑性が移ることは、それら高い階層のコンポーネントをより複雑にして、低い階層のコンポーネントに必要以上の柔軟性を強制します。

### Error Boundary
UI の一部に JavaScript エラーがあってもアプリ全体が壊れてはいけません。React ユーザがこの問題に対応できるように、React 16 では “error boundary” という新しい概念を導入しました。  

error boundary は**自身の子コンポーネントツリーで発生した JavaScript エラーをキャッチし、エラーを記録し、**クラッシュしたコンポーネントツリーの代わりに**フォールバック用の UI を表示する** React コンポーネントです。    error boundary は配下のツリー全体のレンダー中、ライフサイクルメソッド内、およびコンストラクタ内で発生したエラーをキャッチします。  

error boundary は以下のエラーをキャッチしません：

- イベントハンドラ（詳細）
- 非同期コード（例：setTimeout や requestAnimationFrame のコールバック）
- サーバサイドレンダリング
- （子コンポーネントではなく）error boundary 自身がスローしたエラー

error boundary はコンポーネントに対して JavaScript の catch {} ブロックのように動作します。error boundary になれるのはクラスコンポーネントだけです。実用上、一度だけ error boundary を定義してそれをアプリケーションの至るところで使用することがよくあります。

error boundary は配下のツリー内のコンポーネントで発生したエラーのみをキャッチすることに注意してください。error boundary は自身で起こるエラーをキャッチできません。error boundary がエラーメッセージのレンダーに失敗した場合、そのエラーは最も近い上位の error boundary に伝搬します。この動作もまた、JavaScript の catch {} ブロックの動作と似ています。  

### ref のフォワーディング
ref のフォワーディングはあるコンポーネントを通じてその子コンポーネントのひとつに ref を自動的に渡すテクニックです。これは基本的にはアプリケーション内のほとんどのコンポーネントで必要ありません。しかし、コンポーネントの種類によっては、特に再利用可能なコンポーネントライブラリにおいては、便利なものとなるかもしれません。  

#### DOM コンポーネントに ref をフォワーディングする
```jsx
function FancyButton(props) {
  return (
    <button className="FancyButton">
      {props.children}
    </button>
  );
}
```

React コンポーネントは、レンダーの結果も含め、実装の詳細を隠蔽します。FancyButton を使用する他のコンポーネントは内側の button DOM 要素に対する ref を取得する必要は通常ありません 。これは、互いのコンポーネントの DOM 構造に過剰に依存することを防ぐので、良いことです。  

そういったカプセル化は FeedStory や Comment のようなアプリケーションレベルのコンポーネントでは望ましいことではありますが、FancyButton や MyTextInput といった非常に多くのところで再利用可能な “末梢の” コンポーネントでは不便である可能性があります。このようなコンポーネントは、アプリケーションのいたるところで通常の DOM である button や input と同様に扱われる傾向にあり、フォーカス、要素の選択、アニメーションをこなすにはそれら DOM ノードにアクセスすることが避けられないかもしれません。  

**ref のフォワーディングはオプトインの機能であり、それにより、コンポーネントが ref を受け取って、それをさらに下層の子に渡せる（つまり、ref を “転送” できる）ようになります。**  

```jsx
const FancyButton = React.forwardRef((props, ref) => (
  <button ref={ref} className="FancyButton">
    {props.children}
  </button>
));

// You can now get a ref directly to the DOM button:
const ref = React.createRef();
<FancyButton ref={ref}>Click me!</FancyButton>;
```

1. React.createRef を呼び、React ref をつくり、それを ref 変数に代入します。
2. ref を `<FancyButton ref={ref}>` に JSX の属性として指定することで渡します。
3. React は ref を、forwardRef 内の関数 (props, ref) => ... の 2 番目の引数として渡します。
4. この引数として受け取った ref を `<button ref={ref}>` に JSX の属性として指定することで渡します。
5. この ref が紐付けられると、ref.current は `<button>` DOM ノードのことを指すようになります。


2 番目の引数 ref は React.forwardRef の呼び出しを使ってコンポーネントを定義したときにだけ存在します。通常の関数またはクラスコンポーネントは ref 引数を受け取らず、ref は props からも利用できません。  

### フラグメント
React でよくあるパターンの 1 つに、コンポーネントが複数の要素を返すというものがあります。フラグメント (fragment) を使うことで、DOM に余分なノードを追加することなく子要素をまとめることができるようになります。  

```jsx
class Columns extends React.Component {
  render() {
    return (
      <>
        <td>Hello</td>
        <td>World</td>
      </>
    );
  }
}
```

フラグメントを使用すると、以下のように正しい出力になる
```jsx
<table>
  <tr>
    <td>Hello</td>
    <td>World</td>
  </tr>
</table>
```

仮に`<div>`で囲んだ場合こうなる

```jsx
<table>
  <tr>
    <div>
      <td>Hello</td>
      <td>World</td>
    </div>
  </tr>
</table>
```

明示的に `<React.Fragment>` と宣言したフラグメントでは key を持つことができます。これはコレクションをフラグメントの配列に変換するときに有用です。たとえば定義リストを作成する時に利用します：  
```jsx
function Glossary(props) {
  return (
    <dl>
      {props.items.map(item => (
        // Without the `key`, React will fire a key warning
        <React.Fragment key={item.id}>
          <dt>{item.term}</dt>
          <dd>{item.description}</dd>
        </React.Fragment>
      ))}
    </dl>
  );
}
```

key はフラグメントに渡すことができる唯一の属性です。将来的には、イベントハンドラのような他の属性を渡すこともサポートするかもしれません。












