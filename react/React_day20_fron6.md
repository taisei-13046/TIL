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
