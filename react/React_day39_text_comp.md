## やったこと
Textのコンポーネントを作成した。

### PropsにHTMLElementの型を継承する
```ts
interface Props extends HTMLAttributes<HTMLDivElement>
```
[reactの型定義ファイル index.d.ts](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/1349b640d4d07f40aa7c1c6931f18e3fbf667f3a/types/react/index.d.ts#L2002)  

HTMLAttributesの型は以下のようになっている  

```ts
interface HTMLAttributes<T> extends AriaAttributes, DOMAttributes<T> {
    // React-specific Attributes
    defaultChecked?: boolean;
    defaultValue?: string | number | string[];
    suppressContentEditableWarning?: boolean;
    suppressHydrationWarning?: boolean;

    // Standard HTML Attributes
    accessKey?: string;
    className?: string;
    contentEditable?: Booleanish | "inherit";
    contextMenu?: string;
    dir?: string;
    draggable?: Booleanish;
    hidden?: boolean;
    id?: string;
    lang?: string;
    placeholder?: string;
    slot?: string;
    spellCheck?: Booleanish;
    style?: CSSProperties;
    tabIndex?: number;
    title?: string;
    translate?: 'yes' | 'no';

    // Unknown
    radioGroup?: string; // <command>, <menuitem>

    // WAI-ARIA
    role?: string;

    // RDFa Attributes
    about?: string;
    datatype?: string;
    inlist?: any;
    prefix?: string;
    property?: string;
    resource?: string;
    typeof?: string;
    vocab?: string;

    // Non-standard Attributes
    autoCapitalize?: string;
    autoCorrect?: string;
    autoSave?: string;
    color?: string;
    itemProp?: string;
    itemScope?: boolean;
    itemType?: string;
    itemID?: string;
    itemRef?: string;
    results?: number;
    security?: string;
    unselectable?: 'on' | 'off';

    // Living Standard
    /**
     * Hints at the type of data that might be entered by the user while editing the element or its contents
     * @see https://html.spec.whatwg.org/multipage/interaction.html#input-modalities:-the-inputmode-attribute
     */
    inputMode?: 'none' | 'text' | 'tel' | 'url' | 'email' | 'numeric' | 'decimal' | 'search';
    /**
     * Specify that a standard HTML element should behave like a defined custom built-in element
     * @see https://html.spec.whatwg.org/multipage/custom-elements.html#attr-is
     */
    is?: string;
}
```
つまりこれらのHTML要素としての型がPropsに継承される  

### styled-componentsは`as`で上書きすることができる
[Extending Styles](https://styled-components.com/docs/basics#extending-styles)  
```tsx
const Button = styled.button`
`;

const TomatoButton = styled(Button)`
`;

render(
  <div>
    <Button>Normal Button</Button>
    <Button as="a" href="#">Link with Button styles</Button>
    <TomatoButton as="a" href="#">Link with Tomato Button styles</TomatoButton>
  </div>
);
```
#### `as`の型定義
[as prop of styled components in TypeScript](https://github.com/emotion-js/emotion/issues/1137)  
```ts
interface StyledLinkProps {
  as?: React.ElementType;
}
```

### createGlobalStyleでGlobalにスタイルを当てる
[createGlobalStyle](https://styled-components.com/docs/api#createglobalstyle)  
sample
```tsx
import { createGlobalStyle } from 'styled-components'

const GlobalStyle = createGlobalStyle`
  body {
    color: ${props => (props.whiteColor ? 'white' : 'black')};
  }
`

// later in your app

<React.Fragment>
  <GlobalStyle whiteColor />
  <Navigation /> {/* example of other top-level stuff */}
</React.Fragment>
```

### CSS helper
[css helper](https://styled-components.com/docs/api#css)  
```tsx
import styled, { css } from 'styled-components'

const complexMixin = css`
  color: ${props => (props.whiteColor ? 'white' : 'black')};
`

const StyledComp = styled.div`
  /* This is an example of a nested interpolation */
  ${props => (props.complex ? complexMixin : 'color: blue;')};
`
```

css helperを使用することでcssの構文ミスや不適切な要素がstyledComponent内に入ってきてもエラーを起こすことができる  

### TypeScriptの組み込み型関数 `Record`
[Record<Keys, Type>](https://www.typescriptlang.org/docs/handbook/utility-types.html#recordkeys-type)  
[サバイバルTypeScript](https://typescriptbook.jp/reference/type-reuse/utility-types/record)  

Record<Keys, Type>はプロパティのキーがKeysであり、プロパティの値がTypeであるオブジェクト型を作るユーティリティ型  
**Keysに代入できる型は、string、number、symbolとそれぞれのリテラル型**  
Typeには任意の方を指定できる  

```ts
type StringNumber = Record<string, number>;
const value: StringNumber = { a: 1, b: 2, c: 3 };
```

### YAGNI原則(You ain't gonna need it.)  
[ソフトウェア設計についての原則や法則についてまとめてみた](https://zenn.dev/nanagi/articles/0e899711611630)  
予測による実装が実際に役立つことは少ないという経験則から生まれた原則  
YAGNIに従うべき理由  
- 必要最低限の実装で済ますことでコードがシンプルに保たれ、予期しない変更に対応しやすくなる
- プロジェクトの状況は刻一刻と変化するため、予測に基づく実装が修正なしに役立つことが少ない  
- 将来に備えた実装を行うと一般的にはコードの量は増えるが、コード量の増加は保守コストを引き上げる傾向がある
- プロジェクトにおける優先順位を重視するべきであり、いつ役立つかわからないものにコストをかけるというのは優先順位の観点から適切でないことが多い
- 将来に備えた実装を行うと単純に実装コストが増えてリードタイムが増える傾向がある
- メンバーは学習・成長するため、プロジェクトの初期に書いたコードよりも後に書かれたコードほど品質が良くなる傾向がある


### KISSの原則 (Keep it simple stupid.）
「単純で馬鹿なコードに保ちなさい！」  
つまり、この原則は賢すぎる複雑なコードは好ましくないということ  





