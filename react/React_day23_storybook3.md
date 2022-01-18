## やったこと
React Storybookのpractice  

## 実際にReact TS Storybookを触ってみる
とりあえず触るのが一番早いかなと  

```shell
npx -p @storybook/cli sb init
```

何はともあれinstallから  

すると、すでに`stories/`に幾つかファイルが作られている  
これらを一個ずつ見ていきたい  

初めに`stories/Button.tsx`

上部でPropsとして渡される引数の型を宣言している  
```ts
interface ButtonProps {
  primary?: boolean;
  backgroundColor?: string;
  size?: 'small' | 'medium' | 'large';
  label: string;
  onClick?: () => void;
}
```

上で定義したPropsの型を渡して引数を受け取っている
```ts
export const Button = ({
  primary = false,
  size = "medium",
  backgroundColor,
  label,
  ...props
}: ButtonProps) => {
};
```
- default引数を`=`で指定している
- `...props`は`onClick`を受け取っているが、直接書かないのはなんでだろう？
  - 後で書くときに記述が減るからかなと推測した

```ts
export const Button = ({/* 省略 */}: ButtonProps) => {
  const mode = primary
    ? "storybook-button--primary"
    : "storybook-button--secondary";
  return (
    <button
      type="button"
      className={["storybook-button", `storybook-button--${size}`, mode].join(
        " "
      )}
      style={{ backgroundColor }}
      {...props}
    >
      {label}
    </button>
  );
};
```
- `  const mode = primary? "storybook-button--primary": "storybook-button--secondary";`
  - 三項演算子でstyleに渡す前に確定させておく

流れはこのような感じ。  
では、storiesの方はどうなっているのか？  

上から読み解いていく  
```ts
import { ComponentStory, ComponentMeta } from '@storybook/react';

export default {
  title: 'Example/Button',
  component: Button,
  // More on argTypes: https://storybook.js.org/docs/react/api/argtypes
  argTypes: {
    backgroundColor: { control: 'color' },
  },
} as ComponentMeta<typeof Button>

// More on component templates: https://storybook.js.org/docs/react/writing-stories/introduction#using-args
const Template: ComponentStory<typeof Button> = (args) => <Button {...args} />;
```
まず、`title`と`component`は必須で指定する必要がある  
### argTypesって何？
[argTypes 公式doc](https://storybook.js.org/docs/react/api/argtypes)  
これはStorybookのControlsの機能の一部?である  
#### Controlsとは
Controlsを用いると、Storybook上でコンポーネントのpropsの値を動的に変更し、UIを確かめることができるもの  
さまざまな指定の仕方があるうちの一つがargTypesである 

```ts
argTypes: {
  backgroundColor: { control: 'color' },
},
```
わかりやすかった資料  
[StorybookのControlsを試してみた](https://blog.web.nifty.com/engineer/3540)  
`backgroundColor: { control: 'color' },`を指定すると色を好きな色に変更ができるようになる  

### `ComponentStory, ComponentMeta`この型って何?
```ts
export declare type ComponentMeta<
  T extends keyof JSX.IntrinsicElements | JSXElementConstructor<any>
> = Meta<ComponentProps<T>>;

export declare type ComponentStory<
  T extends keyof JSX.IntrinsicElements | JSXElementConstructor<any>
> = Story<ComponentProps<T>>;
```
型の定義は上のようになっている  
**ComponentMeta は Meta のエイリアス、ComponentStory は Story のエイリアスである**  
**ComponentMeta, ComponentStory を使う場合は型引数が必須となり、この時渡せるのは story のベースとなるコンポーネントの型のみ**  
**これらの新しい型の登場によって、 Storybook のためだけにコンポーネントの props を export する必要がなくなった**  

### そもそも`Meta, Story`って何？？
**Metaについて**  
storiesファイルでは、Storybookのメタデータをdefault exportする必要がある  
```ts
const meta: Meta<Props> = {
  title: 'Component', // カタログのタイトルになる部分。 スラ(/)で区切ると階層ができる。
  component: Component, // ストーリーブック化するComponent
};

export default meta;
```
**Storyについて**  
ストーリーを作成するには、名前つきexportを用いる。  
変数名がそのままStorybookに表示される。
```ts
export const Default: Story = () => <Component />;
```
参考  
[ComponentをStorybookで管理する方法](https://qiita.com/cheez921/items/0a843aa1dcf897ff025a)  

これらをそれぞれ`ComponentStory, ComponentMeta`を使うと簡略化することができる  

```ts
export const Primary = Template.bind({});
// More on args: https://storybook.js.org/docs/react/writing-stories/args
Primary.args = {
  primary: true,
  label: 'Button',
};
```
このように記述してexportすることでストーリーを表示することができる  

## 実際にstyled-componentsを使ってコンポーネントを作ってみる
`src/components/atoms/Text/`に`index.tsx`と`Text.stories.tsx`を作る  
フォルダ構成は以下のよう
```
src - components
      - atoms
        - Text
          - index.tsx
          - Text.stories.tsx
    - style_vars
      - mixins
        - text.ts
      - properties
        - font-size.ts
      - index.ts
```

### 2. Textのコンポーネントを作成する
```ts
import React from "react";
import styled from "styled-components";

import SV from "../../../style_vars";

export const TextExample = () => {
  return <StyledText text="sample-text" textStyle={SV.TEXT.H1} />
};

export default TextExample;

type TextProps = {
  className?: string;
  text: string;
  textStyle: string;
};

const Text = (props: TextProps) => {
  return <div className={props.className}>{props.text}</div>;
};

const StyledText = styled(Text)`
  ${(props) => props.textStyle}
  margin-bottom: 12px;
`;
```
今まで、muiのコンポーネントなどを
```ts
const SButton = styled(Button)``
```
などの記述はしていたが、自分で作ったTextコンポーネントをstyled-componentsに当てることがなかったので発見  
