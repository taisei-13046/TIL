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

