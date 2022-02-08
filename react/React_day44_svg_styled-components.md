## やったこと
案件作業を進めた

### SVGについて
意外に今まで知らなかったSVG  
それぞれの渡せる要素についてまとめてみた  
まあ読むのはお決まりのMDNですよね！   
[SVG: Scalable Vector Graphics](https://developer.mozilla.org/ja/docs/Web/SVG)  

Scalable Vector Graphics (SVG) は、二次元ベースのベクター形式のための XML に基づくマークアップ言語です。  
旧来の JPEG や PNG のようなビットマップ画像形式と比較して、 SVG 形式のベクター画像は、品質を損なうことなく任意の大きさでレンダリングすることができ、テキストを更新することで、グラフィックエディターを使用せずに簡単にローカライズすることができます。  

案件で使用されていた要素を調べた  

#### ViewBox
[SVGのviewBoxをわかりやすく紐解く](https://www.indetail.co.jp/blog/13002/)  
viewBoxを指定することで、描画エリアのアスペクト比、およびその中の要素の相対的なサイズを決定します。  
`viewBox="x, y, width, height"`  

#### fill-rule
fill-rule 属性は複数のパスで囲まれる部分がパスの内側かどうかを判断して塗りつぶしを制御するための属性で、値としては主に nonzero (省略した場合のデフォルト値) と evenodd があります。  
`fill-rule="nonzero" (省略時のデフォルト値)`  

#### clip-rule
`<clipPath>`要素内に含まれるgraphics要素にのみ適用されます。clip-rule属性は、基本的にfill-rule属性と同じ働きをしますが、`<clipPath>`定義に適用される点が異なります。  

#### fill
fill 属性には使われ方により2つの意味があります.  1つは図形やテキストに使われた場合で，その要素を塗りつぶす色を意味します．もう1つはアニメーションに使われた場合で，そのアニメーションの最終状態を定義します．  

#### stroke
stroke属性は与えられた図形要素の外側に描画される色を定義します。stroke属性のデフォルト値は none です.  

#### preserveAspectRatio
preserveAspectRatio 属性は、与えられたアスペクト比を提供するビューボックスを持つ要素が、異なるアスペクト比を持つビューポートにどのように適合しなければならないかを示すものである。  
svgタグには、何も指定しなくてもデフォルトで、`preserveAspectRatio="xMidYMid meet"`が指定されています。




参考資料 [SVGの記述方法](https://qiita.com/takeshisakuma/items/777e3cb0a54ea7b1dbe7)  


## styled-componentsのドキュメントを読みたくなった
### Basic
#### Extending Styles
```tsx
// The Button from the last section without the interpolations
const Button = styled.button`
  color: palevioletred;
  font-size: 1em;
  margin: 1em;
  padding: 0.25em 1em;
  border: 2px solid palevioletred;
  border-radius: 3px;
`;

// A new component based on Button, but with some override styles
const TomatoButton = styled(Button)`
  color: tomato;
  border-color: tomato;
`;

render(
  <div>
    <Button>Normal Button</Button>
    <Button as="a" href="#">Link with Button styles</Button>
    <TomatoButton as="a" href="#">Link with Tomato Button styles</TomatoButton>
  </div>
);
```
- `styled()`の形で既存のスタイルを拡張することができる
- `as`propsを使うことによって、要素を変更することができる

**`as`propsの補足**  
```tsx
const Button = styled.button`
  display: inline-block;
  color: palevioletred;
  font-size: 1em;
  margin: 1em;
  padding: 0.25em 1em;
  border: 2px solid palevioletred;
  border-radius: 3px;
  display: block;
`;

const ReversedButton = props => <Button {...props} children={props.children.split('').reverse()} />

render(
  <div>
    <Button>Normal Button</Button>
    <Button as={ReversedButton}>Custom Button with Normal Button styles</Button>
  </div>
);
```
カスタムコンポーネントを`as`として渡すことも可能  
v4以前は`withComponent`もあったが今は非推奨  

#### Passed props
```tsx
// Create an Input component that'll render an <input> tag with some styles
const Input = styled.input`
  padding: 0.5em;
  margin: 0.5em;
  color: ${props => props.inputColor || "palevioletred"};
  background: papayawhip;
  border: none;
  border-radius: 3px;
`;

// Render a styled text input with the standard input color, and one with a custom input color
render(
  <div>
    <Input defaultValue="@probablyup" type="text" />
    <Input defaultValue="@geelen" type="text" inputColor="rebeccapurple" />
  </div>
);
```
上の例で見ると、`defaultValue`, `type`はDOMに渡されるが、`inputColor`はpropsに渡されてもDOMには渡されない  
意図的にpropsを伝搬させたくない場合には、2つの方法がある  
[Props Are Not Forever: Preventing Props From Being Passed to the DOM with styled-components v5.1](https://dev.to/sarahscode/props-are-not-forever-preventing-props-from-being-passed-to-the-dom-with-styled-components-v5-1-l47)  

[Transient props v5.1](https://styled-components.com/docs/api#transient-props)  

```tsx
const Comp = styled.div`
  color: ${props =>
    props.$draggable || 'black'};
`;

render(
  <Comp $draggable="red" draggable="true">
    Drag me!
  </Comp>
);
```
transient propsでは、`$`をつけることによって明示的に伝搬させないようにできる  

[Introducing “transient” props](https://medium.com/@probablyup/introducing-transient-props-f35fd5203e0c)  
もっとわかりやすい記事があった  
要約する  

```tsx
import styled from "styled-components";
const SomeOtherComponent = props => <div {...props} />;
const Comp = styled(SomeOtherComponent)`
  color: ${p => p.color};
`;
<Comp color="blue">Hello world!</Comp>
```

このようなコンポーネントの場合、propsはDOMにまで伝搬されてしまう。  
ただし、`styled.div`のようにHTML要素をstyledで指定した場合にはこれはおき得ない。  
現状では、color propはHTML属性としてDOMに残ってしまう  
その場合は以下のように`$`をつけることによって、propsの伝搬を最上位に絞って渡すことができる  
```tsx
import styled from "styled-components";
const SomeOtherComponent = props => <div {...props} />;
const Comp = styled(SomeOtherComponent)`
  color: ${p => p.$color};
`;
// $color will not be passed to SomeOtherComponent
<Comp $color="blue">Hello world!</Comp>
```
仮に同じ名前のpropsが渡されて、片方がtransient化されていた場合
```tsx
import styled from "styled-components";
const OtherComp = ({ as: asProp, ...props }) => <asProp {...props} onClick={somethingCool} />;
const Comp = styled.div`
  color: red;
`;
// Now it's a <span> element with an onClick handler
// that does something cool
<Comp $as={OtherComp} as="span">Hello World!</Comp>
```
transient化されたpropsを消費して、通常のpropsを伝搬する  

参考資料  
[React does not recognize the prop passed to a styled-component within Material UI](https://stackoverflow.com/questions/61488512/react-does-not-recognize-the-prop-passed-to-a-styled-component-within-material-u)  





