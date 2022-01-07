## やったこと
今日もfrontの課題を進めた

その前にReactの記事で興味深いものがあった  
それが[useMemoのコストを心配する前に余計なdivを減らせ！](https://zenn.dev/uhyo/articles/usememo-time-cost)である  

### useMemoのコストを心配する前に余計なdivを減らせ！
React では、useMemoやReact.memoなどが最適化の手段である  
これらは、`最適化`をするためのものなので、むやみやたらに使うと逆にパフォーマンスを下げかねない  
しかし、**あるコンポーネントが一つ余計なuseMemoを持っているよりも、一つ余計な`<div>`をレンダリングする方が、  
パフォーマンス（レンダリングにかかる時間）をより悪化させるという事実がある**  
したがって、useMemoなどを減らすことに執心する場合は、それと同等以上の熱量で余計な要素を減らすことに執心するのが良い  
<img width="278" alt="スクリーンショット 2022-01-07 10 17 34" src="https://user-images.githubusercontent.com/78260526/148475447-d119fa0c-896d-4011-9f55-8443e7823655.png">  

### SVGタグ
svg 要素は、新しい座標系とビューポートを定義するコンテナ  
SVG ドキュメントの最も外側の要素として使用されますが、SVG または HTML ドキュメントの中に SVG フラグメントを埋め込むためにも使用できる  

#### svgとは
SVG は Scalable Vector Graphics の略で、二次元グラフィックスを XML で記述するための言語（テキストファイル）であり、ベクター形式の画像フォーマット  

### marginの複数指定
- 1つ: 四方向同じ
- 2つ: 上下、左右
- 3つ: 上、左右、下
- 4つ: 上、右、下、左

## Molecules/card
### なんであれで三角形ができてるの？
[BORDERだけで三角形ができる仕組み](https://www.granfairs.com/blog/staff/make-triangle-with-css)  

#### borderの特性を知る
```css
.triangle{
  width: 100px;
  height: 100px;
  border-top: 10px solid #F0897F;
}
```
上のCSSの場合、結果は
![スクリーンショット 2022-01-07 13 08 14](https://user-images.githubusercontent.com/78260526/148490082-815a1d1c-40f0-4d44-a5a6-d26095806ab0.png)  
このようになる  

そして、右にborderを追加すると
```css
.triangle{
  width: 100px;
  height: 100px;
  border-top: 10px solid #F0897F;
  border-right: 10px solid #f6da69;
}
```
![スクリーンショット 2022-01-07 13 08 56](https://user-images.githubusercontent.com/78260526/148490132-7fee98b2-00aa-4227-b571-09d62b4e334b.png)  

結果は上のようになる  
ここで注目するのは、border同士の接地面が**斜め**になっていること  
![スクリーンショット 2022-01-07 13 11 18](https://user-images.githubusercontent.com/78260526/148490321-b40347f9-3bcb-42be-aca6-165e83eb867b.png)  
あとはどこを透明色にするか次第で三角形の向きが決まる  


## hackason
### JSXでDate型を表示したい
結論: Date型はobjectなので表示できない  
それを`toString()`で変換することで表示できる  

また、`moment`のパッケージをinstallしたところ、めちゃくちゃ簡単にDateをformat指定できた  
参考  
- [Reactで時間を表示。momentで日付操作](https://kazunaka.com/js-moment/?utm_source=rss&utm_medium=rss&utm_campaign=js-moment#outline__7)  
- [momentのformat](https://momentjs.com/docs/#/displaying/format/)  


### useContext
React TSでuseContext, createContextを使うときの型指定がややこしかった  
useStateの関数を指定する時の型
```js
setCount: Dispatch<SetStateAction<number>>
```
では
```js
const CountContext = React.createContext()
```
と初期値の指定は必須ではなかったが  
TypeScriptでは初期値を必ず設定してあげる必要がある  

参考資料
[useContextとuseStateを組み合わせ](https://qiita.com/Rascal823/items/0f53ffbb410505b707f8)  
