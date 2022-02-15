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























