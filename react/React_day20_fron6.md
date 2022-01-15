## やったこと
フロントの最終課題を進め

### 2行目で文字列を折り畳みたいな〜
![スクリーンショット 2022-01-15 10 37 57](https://user-images.githubusercontent.com/78260526/149604008-bd96f41b-5a1d-4b2d-b72c-288e5f2aff0b.png)  
こういう感じ  

１行であれば`text-overflow: ellipsis;`を使えば実現できる  
```css
width: 400px;
white-space: nowrap;
overflow: hidden;
text-overflow: ellipsis;
```
- `white-space`: 要素内のホワイトスペースをどのように扱うかを設定する
  - [white-space](https://developer.mozilla.org/ja/docs/Web/CSS/white-space)
- `overflow`: 要素の内容が多すぎてブロック整形コンテキストに収まらない場合にどうするかを設定する
  - [over-flow](https://developer.mozilla.org/ja/docs/Web/CSS/overflow)
- `text-overflow`: 非表示のあふれた内容をどのようにユーザーに知らせるのかを設定する
  - 切り取られるか、省略記号 ('…') を表示するか、独自の文字列を表示するか
  - [text-overflow](https://developer.mozilla.org/ja/docs/Web/CSS/text-overflow)

widthを指定しないといけないのはなんか気持ち悪いな〜とは思ってる  

では、2行以上になった場合はどうするか。  
```css
-webkit-box-orient: vertical;
-webkit-line-clamp: 2;
```
で解決する  

気になった。 `-webkit`ってなんだ。  
[[CSS] -webkit-ってなに？](https://code-schools.com/css-webkit/)  
webkitとはベンダープレフィックスと呼ばれCSS3で実装予定の機能をブラウザ各社が先行して実装した機能を各ブラウザで使えるようにしたもの  
各ブラウザベンダーの対応は以下の通り。  

- -o- : opera
- -ms- : IE(Microsoft)
- -moz- : firefox(mozilla社)
- -webkit- : Chrome, Safari

つまり、Chrome, Sfariで先行して使えるようにしたcssのこと

- `box-orient`: 要素がその中身をレイアウトする方向が、水平か垂直かを指定する
  - [box-orient](https://developer.mozilla.org/ja/docs/Web/CSS/box-orient)
- `line-clamp`: ブロックコンテナーのコンテンツを指定した行数に制限する
  - [line-clamp](https://developer.mozilla.org/ja/docs/Web/CSS/-webkit-line-clamp)


## Storybook * Reactのチュートリアル
[React 向け Storybook のチュートリアル](https://storybook.js.org/tutorials/intro-to-storybook/react/ja/get-started/)  
Storybookの使用についての理解がとても曖昧なので、チュートリアルをやってみる  
特にReactでStoryBookを使ったことがない  

初めに.envファイルを作成して
```
SKIP_PREFLIGHT_CHECK=true
```

の記述をする  
この記述がないと、create-react-appとstorybookの間でerrorが起きてしまうよう  

storybookの追加は以下のコマンドを使用  
```
npx -p @storybook/cli sb init
```

