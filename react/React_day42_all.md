## やったこと

## レビューまとめ
### COLOR定数をまとめる方法(一例)
```ts
type HexColor = `#${string}`

interface AppColorSet {
  primary: HexColor
  white: HexColor
  black: HexColor
}

interface AppTheme {
  color: AppColorSet
}

export const theme: AppTheme = {
  color: {
    primary: "#00A559",
    white: "#fff",
    black: "#4D4D4D",
  },
} as const
```
`HexColor`は色の方をliteral typeで指定  
`AppColorSet`で必要な種類を指定  
```ts
  color: {
    primary: "#00A559",
    white: "#fff",
    black: "#4D4D4D",
  },
```
この構造の型を作成する  

また、`ThemeProvider`に入れ込むことで下位コンポーネントで使用可能になる  
```ts
background-color: ${(props) => props.isSelected ? props.theme.color.primary : props.theme.color.white};
```
受け取る時はpropsに格納されている  

### StorybookでThemeProviderを使う方法



