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
```ts
import React from 'react';
import './button.css'

interface ButtonProps {
  /**
   * Is this the principal call to action on the page?
   */
  primary?: boolean;
  /**
   * What background color to use
   */
  backgroundColor?: string;
  /**
   * How large should the button be?
   */
  size?: 'small' | 'medium' | 'large';
  /**
   * Button contents
   */
  label: string;
  /**
   * Optional click handler
   */
  onClick?: () => void;
}

/**
 * Primary UI component for user interaction
 */
export const Button = ({
  primary = false,
  size = 'medium',
  backgroundColor,
  label,
  ...props
}: ButtonProps) => {
  const mode = primary ? 'storybook-button--primary' : 'storybook-button--secondary';
  return (
    <button
      type="button"
      className={['storybook-button', `storybook-button--${size}`, mode].join(' ')}
      style={{ backgroundColor }}
      {...props}
    >
      {label}
    </button>
  );
};
```

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

```ts
import React from 'react';
import { ComponentStory, ComponentMeta } from '@storybook/react';

import { Button } from './Button';

// More on default export: https://storybook.js.org/docs/react/writing-stories/introduction#default-export
export default {
  title: 'Example/Button',
  component: Button,
  // More on argTypes: https://storybook.js.org/docs/react/api/argtypes
  argTypes: {
    backgroundColor: { control: 'color' },
  },
} as ComponentMeta<typeof Button>;

// More on component templates: https://storybook.js.org/docs/react/writing-stories/introduction#using-args
const Template: ComponentStory<typeof Button> = (args) => <Button {...args} />;

export const Primary = Template.bind({});
// More on args: https://storybook.js.org/docs/react/writing-stories/args
Primary.args = {
  primary: true,
  label: 'Button',
};

export const Secondary = Template.bind({});
Secondary.args = {
  label: 'Button',
};

export const Large = Template.bind({});
Large.args = {
  size: 'large',
  label: 'Button',
};

export const Small = Template.bind({});
Small.args = {
  size: 'small',
  label: 'Button',
};
```

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
