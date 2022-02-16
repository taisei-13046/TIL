## やったこと
SEOについて

とその前に、
## rem, emについて
[CSS3のremとemの違いについて](https://qiita.com/masarufuruya/items/bb40d7e39f56e6c25f0d)   

初めに、CSSのフォントサイズ指定方法は次の2つある  
- 絶対値: 10pxなら絶対に10px
- 相対値: 親要素のサイズによって可変

#### px
```css
html {
  font-size: 32px;
}

h1 {
  font-size: 64px; /* -> 64px; */
}

span {
  font-size: 20px; /* -> 20px; */
}
```

#### em
**親要素のfont-sizeを基準に大きさを計算する**  

```css
html {
  font-size: 32px;
}

h1 {
  font-size: 2em; /* -> 64px; */
}

span {
  font-size: 0.5em; /* -> 32px; */
}
```

#### %

**emと同じで親要素の相対値**  

```css
html {
  font-size: 32px;
}

h1 {
  font-size: 200%; /* -> 64px; */
}

span {
  font-size: 50%; /* -> 32px; */
}
```

#### rem (root em)

**文書のルート要素、つまりhtml要素のfont-sizeを基準にする**

```css
html {
  font-size: 32px;
}

h1 {
  font-size: 2rem; /* -> 64px; */
}

span {
  font-size: 0.5rem; /* -> 16px; */
}
```











