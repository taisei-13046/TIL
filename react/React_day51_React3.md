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






