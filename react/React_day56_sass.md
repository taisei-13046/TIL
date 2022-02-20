## やったこと
sass記法について復習

### Sass, SASS, SCSSの違い
[SassとSASSとSCSSの違いについて](https://uxmilk.jp/38084)  

##### Sass
CSSを拡張したメタ言語のこと

##### SASS
SASS記法やSASSファイルと書かれていることが多く、Sassの2つの記法のうちの一つ

##### SCSS
SCSS記法やSCSSファイルと書かれていることが多く、Sassの2つの記法のうちのよく使われている記法

最初に作られたのがSASS記法（.sass）で、後に作られたのがSCSS記法（.scss）になり、一般的にSCSS記法の方が広く普及しています。  

#### SASS
括弧ではなく、インデントで依存関係を示します。  
```sass
.article
  .title
    color: #222
```

#### SCSS
SCSS記法はネスト（入れ子）を主とした記法
```scss
div {
    margin: 0 auto;
    p {
        padding: 20px;
        span {
             font-color: red;
        }
    }
}
```

### cssの記法復習
[styled-components の :&>before(記号) まとめ](https://blog.ojisan.io/s-c-kigo/)  

#### hoge, fuga
`,` は複数のセレクタを対象にします。  

```css
h1,
h2 {
  color: red;
}
```

#### hoge fuga
スペースは子孫要素を表します。  

```css
div p {
  color: red;
}
```

#### hoge > fuga
`>` は直下要素を表します。

```css
div > p {
  color: red;
}
```

直下は 1 階層だけ下という意味  

```css
<div>
  <p>スタイル当たる</p>
  <form>
    <p>スタイル当たらない</p>
  </form>
</div>
```

#### hoge + fuga
`+` は直後の隣接要素を表します。  

```css
p + span {
  color: orange;
}
```

```html
<p class="hoge">
  一つ内側(p　.hoge)
  <span>二つ内側(span)</span>
</p>
<span>一つ内側(span)(ここにスタイルが当たる)</span>
<span>一つ内側(span)</span>
<span>一つ内側(span)</span>
```

#### hoge ~ fuga
`~` は 後続の隣接要素を表します。 `+` が直後だけだったのに対し、これは後続のもの全てが対象です。

```css
p ~ span {
  color: orange;
}
```

```html
<p class="hoge">
  一つ内側(p　.hoge)
  <span>二つ内側(span)</span>
</p>
<span>一つ内側(span)(ここにスタイルが当たる)</span>
<span>一つ内側(span)(ここにスタイルが当たる)</span>
<span>一つ内側(span)(ここにスタイルが当たる)</span>
```

#### hoge:hover
`:` は擬似クラス(pseudo-class)を表すのに使います。   
MDN の説明を借りるなら、擬似クラスは「**セレクターに付加するキーワードであり、選択された要素に対して特定の状態を指定します。**」です。  

#### hoge::before

`::` は擬似要素(Pseudo-elements)を表すのに使います。   
MDN の説明を借りるなら、擬似要素 Pseudo-elements は**セレクターに付加するキーワードで、選択された要素の特定の部分にスタイル付けできるようにするもの**です。  











