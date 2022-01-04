## やったこと
先日に引き続き、MDNでCSSを学習する  

### グリッドレイアウトについて
グリッドとは、水平方向と垂直方向のラインを集めたもの  
グリッドには通常、列（column）、行（row）、そしてそれぞれの行と列の間のギャップ（通常はガター（gutter）と呼ばれます）がある  

```css
.container {
    display: grid;
    grid-template-columns: 200px 200px 200px;
}
```
上のように表記すると、200pxの列が３つ生成される  

#### fr 単位での柔軟なグリッド
この単位は、グリッドコンテナ内の使用可能スペースの割合を表す  
```css
.container {
    display: grid;
    grid-template-columns: 2fr 1fr 1fr;
}
```
上のように定義すると、横幅が2:1:1の割合になる  

`grid-gap: 20px;`を指定すると  
![スクリーンショット 2022-01-04 10 50 09](https://user-images.githubusercontent.com/78260526/147999108-92493cb8-ef6c-4eaa-a0d9-ee9ec5d9e35d.png)  
このように列間と行間が同時に設定される  
列間のギャップには `grid-column-gap` プロパティ、  
行間のギャップには `grid-row-gap` プロパティを使用する  

#### ラインベースの配置
グリッドは常にラインを持っていて、そのラインは 1 から始まり、文書の書字方向モード（Writing Mode）に関連している  
```css
header {
  grid-column: 1 / 3;
  grid-row: 1;
}

article {
  grid-column: 2;
  grid-row: 2;
}

aside {
  grid-column: 1;
  grid-row: 2;
}
```
![スクリーンショット 2022-01-04 10 59 20](https://user-images.githubusercontent.com/78260526/147999696-ddc5f6c2-7b98-45c1-baa2-ef913deabc64.png)  

Qiita記事にもっとわかりやすいGridについての説明があったのでそれをまとめる  
[CSS Grid Layout を極める！（基礎編）](https://qiita.com/kura07/items/e633b35e33e43240d363)  

