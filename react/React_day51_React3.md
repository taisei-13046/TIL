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













