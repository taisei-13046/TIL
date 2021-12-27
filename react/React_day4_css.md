## やったこと 
CSSに不安を持っていたため、今日からひたすらCSSについて勉強する。  
基本的にはMDNをとにかく読みまくる!!  
どこまで分かってるかもわからないので、超基本的なところからやる。

### CSS の構文
ページ上の `<h1>` 要素の文字を大きく、赤くしたいといった時  
```css
h1 {
    color: red;
    font-size: 5em;
}
```
流石にこれらはわかる  

複数セレクタに焦点を当てる場合
```css
p, li {
    color: green;
}
```

クラスに焦点を当てる場合
```css
.special {
  color: orange;
  font-weight: bold;
}
```

HTML要素セレクタに続けてクラスセレクタが記述されている場合
```css
li.special {
  color: orange;
  font-weight: bold;
}
```

`<li>` 要素の中にある `<em>` 要素を選択する場合
```css
li em {
  color: rebeccapurple;
}
```

状態に基づいてスタイリングする
```css
a:link {
  color: pink;
}

a:visited {
  color: green;
}

a:hover {
  text-decoration: none;
}
```

セレクタとコンビネーターを組み合わせる
```css
/* <article> 要素の内側にある <p> 要素の <span> 要素に焦点を当てるとき  */
article p span { ... }

/* <h1> 要素の直後に来る <ul> 要素の、そのまた直後に来る <p> 要素に焦点を当てるとき */
h1 + ul + p { ... }
```

### CSS の全体像
#### 詳細度
```css
.special {
  color: red;
}

p {
  color: blue;
}
```
上の二つが同時に当てられた場合、`p`セレクターが優先される  
後のスタイルは、それより前のスタイルシートに現れた競合するスタイルを置き換える。これが**カスケード規則**  

#### CSSは大きく2つの部品からなる
`プロパティ`: スタイルに関して変更できる何らかの特徴をあらわす、人間が理解できる識別子です。  
例えば、 font-size, width, background-color などです。  
`値`: 各プロパティには値が割り当てられています。  
この値は、プロパティをどのようにスタイル付けするかを示します。  

#### アット規則
CSS のアット規則 は、 CSS が実行すること、またはそれがどのように動作するべきかの指示を提供する
`@import` はスタイルシートを別の CSS スタイルシートにインポートする
```css
@import 'styles2.css';
```

`@media` があり、メディアクエリを作成するために使用される
@import 'styles2.css';
```css
body {
  background-color: pink;
}

@media (min-width: 30em) {
  body {
    background-color: blue;
  }
}
```
