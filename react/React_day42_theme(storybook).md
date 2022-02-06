## やったこと
themeについてのまとめ

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
現在、ThemeProviderが適用されるのは`<ThemeProvider>`で囲われた中のコンポーネントだけ  
すると、Sotrybookではこれらの色の定数は反映されない  
解決方法としては、Storybookの**decorators**にProviderを登録することで解決する  

```tsx
export const StorybookPropvider = (Story: Story, context: StoryContext) => {
  return (
    <ThemeProvider theme={theme}>
      <Story {...context} />
    </ThemeProvider>
  )
}
```
#### decoratorsって何？
ストーリーを描画する際に、そのストーリーをラップした上位コンポーネントのこと  
全てのストーリーに共通して適用するデコレータを定義する場合は、 .storybook/preview.js 内に、 decorators フィールドを追加する  
decorators と、複数形になっていることからもわかるように、ここには配列で複数のデコレータを指定することができます。デコレータが複数ある場合は、定義した順に適用され、ラッパーをラップしたコンポーネントの形になります。  

```js
export const decorators = [
  (Story, context) => ({
    template: `
      <div style="backgroundColor: gray; padding: 10px">
        <Story />
      </div>
    `
  })
]
```
第一引数には、対象のストーリーコンポーネント(`<Story />`)が渡される  
ここでは、 `<story />` を、 `<div style="backgroundColor: gray; padding: 10px">` でラップすることで、全てのストーリーに 10px の padding と背景色を設定しています。  
また、第二引数の context には、描画するストーリーに関する様々なメタデータが含まれています。　　

![スクリーンショット 2022-02-06 14 11 03](https://user-images.githubusercontent.com/78260526/152668518-9a02c73c-643d-4214-a7ec-6aa93202ae72.png)  



