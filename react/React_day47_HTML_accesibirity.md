## やったこと

[HTML と XHTML の達成方法](https://waic.jp/docs/WCAG-TECHS/html.html)  
こいつを読んだ！  

## レビューまとめ
### レビュー依頼の前に！
急いでpushしたコードは実際に穴だらけだったりする。そのため、レビュー依頼前にPRのdiffを確認してから依頼するようにする！  
その際にセルフレビューするのもいいのかなと思った！  
[セルフレビューのコツ ○](https://zenn.dev/sirosuzume/articles/64763fad0efff7)  

### コンポーネント名の命名規則について
明るいLineと暗いLineを作る際に, `LighterLine`か`(色名)Line`かで迷った。  
しかし、色名でいれてしまうと色の定数の名前が変わるたびに変えなくてはいけないので、前者が正しい！  

### styled.d.tsについて
styled-componentsでThemeProviderを使用した際に、styled.d.tsファイルを定義しないと  
1. propsでうまく保管がされない -> DefaultTheme扱いになるから?
2. useTheme()にDefaultThemeが当たっていてasキャストしなくてはいけない

そのため,
```ts
import { AppTheme } from "constants/theme"

declare module "styled-components" {
  // eslint-disable-next-line @typescript-eslint/no-empty-interface
  export interface DefaultTheme extends AppTheme {}
}
```
このようにinterfaceの拡張でDefaultThemeを上書きすることによって、
```tsx
  const theme = useTheme()
```
でthemeを受け取ることができるようになる！！


### H2: 同じリソースに対して隣接する画像とテキストリンクを結合する

![スクリーンショット 2022-02-10 21 55 38](https://user-images.githubusercontent.com/78260526/153412649-e958d4dc-6299-4865-936d-8642d7210678.png)  
```html
 <a href="products.html">
   <img src="icon.gif" alt="">
   Products page
 </a>      
```

このようなコンテンツがあったときに、画像とテキストをそれぞれaタグで囲うのではなく、まとめて囲う  
> このようにすることで、リンクにおける両方の表現が提供されつつも、キーボード利用者にとってリンクは一つだけとなり、ウェブページのリンク先リストを利用者に提供する補助技術も重複リンクを含まなくなる。  

> キーボード利用者の場合、冗長なリンクを通過していくのは面倒である。支援技術の利用者にとって、連続する同様のリンクに遭遇すると混乱する可能性がある。アイコンのテキストによる代替がリンクテキストの複製である場合、スクリーンリーダーは読み上げを 2 度繰り返すことになる。  

### H4: リンク、フォームコントロール、及びオブジェクトを通して、論理的なタブ順序を作成する
ホームページは、キーボードのタブで操作することも考慮して、「タブの順番が整合性の取れた順番になるようにしましょう！」というもの  
HTMLのマークアップが正しければ基本的にはコンテンツの順番どおりの自然なタブでの移動となるはずですが、もし順番を入れ替えていたりしたら、tabindexというHTML属性を使って順番を違和感のないようにする  

> インタラクティブな要素が Tab キーを使ってナビゲートされる時、要素は tabindex 属性の値の増える順にフォーカスが与えられる。0 よりも大きな tabindex 値を持つ要素は、tabindex がない又は 0 の tabindex の要素の前にフォーカスを受けることになる。0 よりも大きな tabindex を持つ全ての要素がフォーカスを受け取った後、残りのインタラクティブな要素はウェブページに現れる順番でフォーカスが与えられる。  

> tabindex 値は、連続した番号である必要はなく、特定の値で始まる必要もない。値は一意的である必要もない。同じ tabindex 値を持つ要素は、その文字の出現順にナビゲートされる。  

### H24: イメージマップの area 要素にテキストによる代替を提供する
「画像上のリンク場所を指し示すarea属性に、そのエリアの目的をテキストで用意しましょう」というもの。

> この達成方法の目的は、イメージマップ上の選択可能領域と同じ役割を果たすテキストによる代替を提供することである。イメージマップは、area 要素によって定義された選択可能領域によって分割されている 1 枚の画像である。各領域は、他のウェブページや、現在のウェブページ内の他の場所へのリンクである。各 area 要素の alt 属性は、その画像の選択可能領域と同じ目的を果たすものである。  

```html
<img src="welcome.gif" usemap="#map1" alt="Areas in the library. Select an area formore information on that area." /> 
```

### H25: title 要素を用いて、ページタイトルを提供する
「titleタグを使いましょう！」という当たり前の内容です。・・・が、対応できていないホームページもあるので、当たり前を大切に。  

> この達成方法の目的は、フレームセットの個々のフレームに含まれるものを含む全ての HTML 及び XHTML ドキュメントにおいて、head 要素のセクション内で title 要素を用いてドキュメントの目的を簡単な文言で定義することである。これにより、ページの本文でオリエンテーション情報を探すことなく、利用者はサイト内の自分のいる場所を素早く見つけることができる。
> ドキュメント内で一度しか現れない (必須の) title 要素は、ほとんど全ての HTML 及び XHTML の要素で利用されるかもしれない title 属性と異なることに注意する。

```html
<html xmlns="http://www.w3.org/1999/xhtml">   
   <head>     
      <title>The World Wide Web Consortium</title>     
   </head>   
   <body>     
      ...   
   </body> 
</html>  
```

### H28: abbr 要素を用いて、略語の定義を提供する
> この達成方法の目的は、abbr 要素を用いることで、略語に対して元の語又は定義を提供することである

> abbr 要素は、頭字語や頭文字語を含むあらゆる略語に用いてよい。HTML4 及び XHTML では、頭文字語や頭字語は acronym 要素を用いてマークアップすることもできる。abbr 要素は、頭字語や頭文字語を含むあらゆる略語に用いてよい。HTML5 以降では、より一般的な abbr 要素の利用が提案され、acronym 要素は廃止されている。

```html
<p>Sugar is commonly sold in 5 <abbr title="pound">lb.</abbr> bags.</p>
<p>Welcome to the <abbr title="World Wide Web">WWW</abbr>!</p>     
```

### H30: a 要素のリンクの目的を説明するリンクテキストを提供する
「リンクに指定する文字はリンク先が伝わるような内容にしましょう」というものです。「もっと見る」というリンクがよくありますが、これでは伝わりません。

「もっと」内容を具体化してあげて、「はにわまんのプロフィールを見る」のようにリンク先の内容がイメージできるものを指定してあげましょう。  

> この達成方法の目的は、リンク先を説明するテキストを a 要素のコンテンツとして提供することにより、リンクの目的を説明することである。その説明により、利用者はこのリンクとウェブページ内にある他のリンクとの違いが区別でき、利用者がリンクをたどるかどうかを決定するのを助ける。一般的に、遷移先の URI では、そのリンクの目的を説明したことにはならない。  

```html
<a href="routes.html">
  Current routes at Boulders Climbing Gym
</a>
```

### H32: 送信ボタンを提供する

フォーム（form）に対して送信ボタンを付けてあげてください。送信ボタンとは具体的には、input type="submit"、input type="image"、 button type="submit"の3つです。

> この達成方法の目的は、利用者がコンテキストの変化を明示的に要求できるメカニズムを提供することである。送信ボタンの使用目的は、フォームに入力されたデータを送信する HTTP リクエストを生成することであるため、コンテキストの変化を起こすために使うものとして適切なコントロールである。

```html
<form action="http://www.example.com/cgi/subscribe/" method="post"><br /> 
 <p>Enter your e-mail address to subscribe to our mailing list.</p><br /> 
 <label for="address">Enter email address:</label><input type="text" 
 id="address" name="address" /> 
 <input type="submit" value="Subscribe" /><br /> 
</form>
```

### H33: title 属性を用いて、リンクテキストを補足する
「title属性でリンクのテキストを補足しましょう」というもの  
この達成方法の目的は、リンクを説明する追加情報を提供するための、a 要素の title 属性の利用方法を示すことである。title 属性は、リンクの目的を明らかにしたり、詳しく説明したりするのに役立つ追加情報の指定に用いる。もし title 属性を通して提供する補足情報が、警告文のように利用者がリンクをたどる前に知っておくべき内容であれば、title 属性ではなくリンクテキストとして提供すべきである。  

```html
<a href="http://example.com/WORLD/africa/kenya.elephants.ap/index.html" 
   title="Read more about failed elephant evacuation">
   Evacuation Crumbles Under Jumbo load
</a>
```

### H34: インラインでテキストの方向を混在させるために、Unicode の right-to-left mark (RLM) 又は left-to-right mark (LRM) を使用する
文字の方向に対して、記号の位置を調整する。具体的には、左から右に読む文字では、&lrm;。右から左に読む文字では&rlm;を記号の後ろの挿入してあげればOK。  
> この達成方法の目的は、HTML の双方向性アルゴリズムが望ましくない結果を生じるとき時に、それを無効にするために Unicode 制御文字の right-to-left mark と left-to-right mark を用いることである。たとえば、スペース又は句読点のようなニュートラルな文字が異なる方向性のテキストの間に置かれている時に必要となることがある。

Unicode 制御文字の right-to-left mark 及び left-to-right mark は、以下に示すように、 そのまま、又は文字符号か数字符号の参照によって挿入することが可能である。

`left-to-right mark (LRM): &lrm; 又は &#x200e; (U+200E)`

`right-to-left mark (RLM): &rlm; 又は &#x200f; (U+200F)`

### H35: applet 要素にテキストによる代替を提供する
applet要素の代替テキストを用意してあげましょう。alt属性とapplet要素のテキストとして存在していることが大切です。  

> この達成方法の目的は、アプレットのラベルを提供する alt 属性を用いること及び applet 要素のボディにテキストによる代替を提供することによって、アプレットに対するテキストによる代替を提供することである。この達成方法では、ユーザエージェントごとに、alt 属性及び applet 要素のボディのサポート状況が異なるため、両方のメカニズムを必要としている。


### H36: 送信ボタンとして用いる画像の alt 属性を使用する
送信ボタンには、input type="image"として指定することが可能です。この送信ボタンは画像になるので、何か書いてあるかをalt属性で指定してあげてください。

> この達成方法の目的は、type 属性が「image」である input 要素において、input 要素の alt 属性を機能的なラベルを提供するのに使用することである。このラベルは、画像の説明をするのではなく、ボタンの機能を説明する。もしそのページに、それぞれ異なる結果につながる複数の送信/実行ボタンがあるならば、ラベルは特に重要である。

```html
<form action="http://example.com/prog/text-read" method="post">
  <input type="image" name="submit" src="button.gif" alt="Submit" />
</form>    
```

### H37: img 要素の alt 属性を使用する
画像が見れない人に対しても、何の画像かをテキストで提示してあげる必要があります。

> mg 要素を使用するときは、簡潔なテキストによる代替を alt 属性に指定する。注記: この属性の値は「ALT テキスト」ともいう。

> 画像がコンテンツを理解するために重要な文字を含むとき、代替テキストにはそれらの文字を含めなくてはならない。これにより、代替テキストはページ上で画像と同じ役割を果たすことができる。 代替テキストは、画像自体の視覚的な特徴を説明する必要はないが、画像と同じ意味を伝えなければらないことに注意する。

```html
<img src="newsletter.gif" alt="Free newsletter. Get free recipes, news, and more. Learn more." />
```











