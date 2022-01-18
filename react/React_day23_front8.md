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
