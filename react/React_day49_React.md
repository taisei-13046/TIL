## やったこと
Reactの公式を復習した  
Main Conceptsは大丈夫！ Advanced guideを見ていく  
[advanced guide](https://ja.reactjs.org/docs/accessibility.html)  

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











