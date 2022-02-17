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


## SEOについて
SEO (Search Engine Optimization, 検索エンジン最適化)は、検索結果上であるウェブサイトをより目立たさせるための過程です。検索ランキングの改善とも言えます。  

SEO の方法は大きく3つに分類されます:

- **技術的な方法**
  - コンテンツをセマンティック HTML を使用してタグ付けすることです。ウェブサイトを探索するとき、クローラーはインデックスに登録させたいコンテンツのみを探してくれるでしょう。
- **文章の作成**
  - サイトの訪問者の語彙を使ってコンテンツを書きましょう。クローラーが主題を理解できるように、画像だけでなくテキストも使用しましょう。
- **サイトの人気**
  - 他の有名なされたサイトがあなたのサイトにリンクしすると、多くのトラフィックを得ることができます。

## roadmapの現状
### 2. HTML, CSS, JS
#### HTML
- Learn the basics -> ⭕️
- Writing Semantic HTML -> ⭕️
- Forms and Validations -> ⭕️
- Conventions and Best Practices -> ⭕️
- Accessibility -> 🔺
- SEO Basics -> ⭕️

#### CSS
- Learn the basics -> ⭕️
- Making Layouts -> ⭕️
- Responsive design and Media Queries -> 🔺

#### JS
- Syntax and Basic Constructs -> ⭕️
- Learn DOM Manipulation -> ⭕️
- Learn Fetch API / Ajax (XHR) -> 🔺
- ES6+ and modular JavaScript -> 🔺
- Understand the concepts Hoisting, Event Bubbling, Scope, Prototype, Shadow DOM, strict -> 🔺




### 1. Fundamental Topics(React Developer)

- create react app -> ⭕️
- JSX -> ⭕️
  - 公式DOCの内容は抑えた
- components
  - Functional components -> ⭕️
  - class components -> 🔺
    - どこまでやろうか迷ってる
- Props vs State -> ⭕️
- Conditional Rendering -> 🔺
- Component Life Cycle -> ⭕️
- Lists and Keys -> ⭕️
- Composition vs Ingeritance -> ⭕️
- Basic Hooks -> ⭕️
  - useState
  - useEffect


## 次のroadmap(Frontend)
### Web Security Knowledge
- HTTPS -> ⭕️
- CORS -> ⭕️
- Content Security Policy -> ❎
- OWASP Security Risks -> ❎

### CSS Preprocessors
- Sass -> 🔺
- PostCSS -> 🔺

### Build Tools 
- Prettier -> ❎
- ESLint -> 🔺
- npm script -> ❎
- webpack -> 🔺
- esbuild -> ❎
- Rollup -> ❎
- Parcel -> ❎
- Vite -> ❎


## 次のRoadmap(React)
### Advanced Topics
- Hooks -> 🔺
- Context ->　⭕️
- Refs -> 🔺
- Render Props -> 🔺
- High Order Components -> 🔺
- Portals -> 🔺
- Error Boundaries -> ❎
- Fiber Architecture -> ❎



