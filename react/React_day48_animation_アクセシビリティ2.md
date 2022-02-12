## やったこと
案件の中で疑問に思ったことを調べる

### レビューまとめ
ハンバーガーメニューを作ってる際に
```tsx
<Container isOpen={isOpen} {...rest}>
  <div />
  <div />
  <div />
</Container>
```
このようなネストした形式になっており、以下のようにcssを当てていた  
```tsx
const Container = styled.button<ContainerProps>`
  display: flex;
  
  div {
    height: ${LINE_HEIGHT}px;
    width: 100%;
    
    :first-child {
    }
    :nth-child(2) {
    }
    :nth-child(3) {
    }
  }
```
しかし、これはstyled-componentsを生かした設計になっていない  

模範例は、
```tsx
<Container isOpen={isOpen} {...rest}>
  <Line />
  <Line />
  <Line />
</Container>
```
```tsx
const Line = styled.div`
  height: ${LINE_HEIGHT}px;
  width: 100%;
`

const Container = styled.button<ContainerProps>`
  display: flex;

  ${Line} {
    }

    :nth-child(2) {
    }

    :nth-child(3) {
    }
  }
`
```
このように指定する方が確かに見やすくstyled-componentsを生かした設計になっているのに納得した！  


### 1. イージングについて
[イージングの基本](https://developers.google.com/web/fundamentals/design-and-ux/animations/the-basics-of-easing?hl=ja)  

- イージングによって、アニメーションはより自然に見えます。
- UI 要素には ease-out アニメーションを使用します。
- ease-in または ease-in-out アニメーションはエンドユーザーに緩慢な印象を与える傾向があるため、継続時間を短くできる場合を除いては、使用を控えてください。

従来のアニメーションでは、ゆっくりと動き出して加速する動きは「スローイン」、高速で動き出して減速する動きは「スローアウト」と呼ばれます。一般的に、ウェブの世界ではこれを「ease in」と「ease out」という専門用語で表現します。これらを組み合わせた「ease in out」という用語が使われることもあります。イージングとは、アニメーションを滑らかで自然に見せるためのプロセスです。

cssで指定できるのは以下の4つ  
- **linear**
- **ease-in**
- **ease-out**
- **ease-in-out**

#### リニア アニメーション
イージングが何もないアニメーションはリニアと呼ばれます。リニア遷移のグラフは次のようになります。  

![スクリーンショット 2022-02-12 10 34 54](https://user-images.githubusercontent.com/78260526/153691120-9adef733-e1fa-4673-8017-62736e4a3a91.png)  

#### ease-out アニメーション
Easing out では、アニメーションはリニアのものよりも高速で動き出し、最後に減速します。

一般的に、ease out はユーザー インターフェースの動作に最適です。動き始めのスピードが速いのでアニメーションの反応が早く感じられ、最後は減速するので自然に見えます。

![スクリーンショット 2022-02-12 10 36 24](https://user-images.githubusercontent.com/78260526/153691180-a166ae73-f29b-48d3-8fae-dd4416c66fd2.png)  

#### ease-in アニメーション
ease-in アニメーションは、動き始めはゆっくりで、最後に速くなります。つまり ease-out の逆です。

この種のアニメーションは、重い石が落下するときの動きに似ています。つまり、ゆっくりと動き始め、最後は勢いよく地面にぶつかって止まります。

ただ、相互作用という観点では、ease-in は唐突に終了するので少し不自然に見えます（現実の世界では、動いている物は突然止まるのではなく、徐々に減速します）。さらに、ease-in には動き出しが重く感じられるという難点があるため、サイトやアプリの反応が悪く見えるおそれがあります。

![スクリーンショット 2022-02-12 10 36 58](https://user-images.githubusercontent.com/78260526/153691193-c5ca5b18-5261-4537-85e3-569b17cecec1.png)  

#### ease-in-out アニメーション
Ease-in-out は、車の加速と減速に似ています。慎重に使用すれば、ease out よりもさらに劇的な効果を実現できます。

アニメーションの継続時間は、極端に長くしないでください。長すぎると、ease-in の効果によってアニメーションの始まりが重く見えます。一般的には 300 ～ 500 ミリ秒の範囲が妥当です。ただし具体的な数値は、そのプロジェクトの雰囲気によって大きく異なります。ease-in-out はゆっくりと立ち上がり、中盤で加速し、ゆっくりと終了するので、全体としてアニメーションのコントラストが強調されるため、ユーザーには好まれると考えられます。

![スクリーンショット 2022-02-12 10 37 46](https://user-images.githubusercontent.com/78260526/153691238-9528d7e2-28c4-4c91-969e-30ba2f739624.png)  


### 2. flex-directionとflex-flow
flex-flowは
- flex-direction
- flex-wrap
を一括指定するプロパティ

#### flex-wrapって何？
flex-wrap は CSS のプロパティで、フレックスアイテムを単一行に押し込むか、あるいは複数行に折り返してもよいかを指定します。折り返しを許可する場合は、行を積み重ねる方向の制御も可能です。  
```css
flex-wrap: nowrap; /* 既定値 */
flex-wrap: wrap;
flex-wrap: wrap-reverse;
```

flex-flowはこれらを一括指定する際に使うため以下のようにできる
```css
flex-flow: row nowrap;
flex-flow: column wrap;
flex-flow: column-reverse wrap-reverse;
```

## 昨日に引き続き、HTMLのアクセシビリティについて
### H44: テキストラベルとフォームコントロールを関連付けるために、label 要素を使用する

> この達成方法の目的は、フォームコントロールとラベルを明示的に関連付けるために、label 要素を利用することである。ラベルは、label 要素の for 属性によって特定のフォームコントロールと結びつけることができる。この場合、for 属性値はフォームコントロールの id 属性値と同じでなければならない。

> id 属性が name 属性と同じ値で、両方とも指定しなければならない場合でも、その id はそのウェブページ内で一意的なものとして、重複して使用してはならない。

明示的なラベルを利用する要素は次の通りである:

- `input type="text"`
- `input type="checkbox"`
- `input type="radio"`
- `input type="file"`
- `input type="password"`
- `textarea`
- `select`

次の場合には、label 要素は利用しない。これらの要素に対するラベルは、value 属性 (送信ボタン及びリセットボタン)、alt 属性 (画像ボタン)、又は要素それ自体の内容 (汎用ボタン) を介して提供されるからである。  
- 送信及びリセットボタン (input type="submit" 又は input type="reset")
- 画像ボタン (input type="image")
- 隠しフィールド (input type="hidden")
- スクリプトボタン (button 要素又は input type="button")

```html
<label for="firstname">First name:</label> 
<input type="text" name="firstname" id="firstname" />
```

### H45: longdesc 属性を用いる
longdescは、役割としては画像のaltに近いですが、altの説明で不十分な場合に、より長い文章（コンテンツ）を外部HTMLファイルに格納して提示する点が異なります。  

> この達成方法の目的は、短いテキストによる代替では画像の機能や情報が十分に伝達できない場合に、longdesc 属性でのファイル指定によって情報を提供することである。longdesc 属性には URI を指定する。これは非テキストコンテンツの詳しい説明を含む目的地を意味する。  

```html
<p><img src="chart.gif" alt="a complex chart" longdesc="chartdesc.html"/></p>
```

### H46: embed 要素と一緒に noembed 要素を用いる
embed要素がサポートされていない場合に表示するコンテンツをnoembed要素で提示してあげる必要があります。

noembed要素はembed要素の内側（小要素）もしくは、直下（兄弟要素）に設置してあげます。  

> この達成方法の目的は、embed 要素の代替コンテンツとして noembed を提供することである。noembed 要素は embed 要素がサポートされていない場合のみレンダリングされる。ページのどこにでも配置できるが、embed 要素の子要素として組み込む方が良い。これによりテキストによる代替が embed 要素に関連付けられてことが支援技術にも明らかになる。  

```html
<embed src="../movies/history_of_rome.mov"
  height="60" width="144" autostart="false">
  <noembed>
    <a href="../transcripts/transcript_history_rome.htm">Transcript of "The history of Rome"</a>
  </noembed>
</embed>
```

### H48: リスト又はリンクのグループに、ol 要素、ul 要素、dl 要素を用いる
「情報をまとめてユーザーにとって理解しやすくするために、リスト系のマークアップをしましょう！」というものです。

見栄えのためのマークアップではない点にご注意ください。

- ul要素 → 順不同の箇条書きリスト
- ol要素 → 順序ありのリスト
- dl要素 → 定義（dt）と説明（dd）がセットになったリスト
情報のまとめ方に応じてリストのマークアップタグを選定してあげてください。

> この達成方法の目的は、リスト要素を役割に合わせて適切に利用し、関連する項目のリストを生成することである。ol 要素は順序のあるリストに、ul 要素は順序のないリストに利用する。定義リスト (dl 要素) は、用語とその定義をまとめるのに利用する。このようなマークアップを用いることで、リストを読みやすくできるとはいえ、すべてのリストをリスト要素でマークアップする必要はない。たとえば、文章に含まれるカンマ区切りのリストを、リスト要素でマークアップする必要はない。


### H49: 強調又は特別なテキストをマークアップするために、セマンティックなマークアップを使用する
セマンティックとは「タグへの意味付け」のような感じで使われます。代表的な要素としては、

- em・・・文字の強調
- strong・・・意味の強調
- cite・・・引用元
- blockquote・・・引用（ブロック）
- quote・・・引用（インライン）
- sub・・・下付き文字

> この達成方法の目的は、プログラムによる解釈が可能にするために、セマンティックマークアップを使用して強調又は特別なテキストをマークアップする方法を示すことである。強調又は特別なテキストをマークアップするためにセマンティックなマークアップを用いることで、その文書に構造を与えることもできる。そうすることで、例えば、ユーザエージェントは、異なるタイプの構造に対して異なる視覚的提示を用いたり、聴覚的提示で異なる声又はピッチを用いたりすることによって、構造を利用者に知覚可能にすることができる。








