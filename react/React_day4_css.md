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
