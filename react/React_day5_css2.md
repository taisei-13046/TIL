## やったこと
frontend教材の最終課題に取り組んだ。  

### Molecules/Messageの作成
- `flex-direction`: **主軸の方向や向き (通常または逆方向) を定義することにより、フレックスコンテナー内でフレックスアイテムを配置する方法を設定する**   
```css
/* 行のテキストの方向に配置 */
flex-direction: row;

/* <row> と同様だが、逆向き */
flex-direction: row-reverse;

/* 積み重なるように配置する */
flex-direction: column;

/* <column> と同様だが、逆向き */
flex-direction: column-reverse;
```
- `line-height`: **テキストの行間を設定するために使用**  
```css
/* 単位のない値: この値を要素のフォントサイズに
掛けたものを使用する */
line-height: 3.5;
```
- `font-weight`: **フォントの太さ (あるいは重み) を指定**

<img width="1023" alt="スクリーンショット 2021-12-28 13 27 11" src="https://user-images.githubusercontent.com/78260526/147528010-610d8974-b041-4d56-adfe-7a8c17df1c8d.png">

<img width="1017" alt="スクリーンショット 2021-12-28 13 27 23" src="https://user-images.githubusercontent.com/78260526/147528015-820b1d53-0871-4b6b-87c6-be95eb216925.png">


### Molecules/NewsAndEventsTitle
#### [margin: 0 auto;で要素が中央ぞろえになる仕組み](https://farm-fruits-riceplant.work/mechanism-margin-zero-auto/)  
**余白を左右均等に割り当てる**ことで要素が中央寄せされている  

#### [marginの正しい理解](https://kojika17.com/2012/08/margin-of-css.html)
![スクリーンショット 2021-12-28 14 02 20](https://user-images.githubusercontent.com/78260526/147529913-3ed3d5d7-b19c-4138-a34d-9f07a18d2110.png)  
**margin と auto と width の関係**
- marginに対して、autoを設定した場合は数値は0になる
- しかし、横幅(width)を指定した状態で、marginの左右のどちらかにautoを指定すると、指定した方に数値を算出する

**marginの相殺**  
特定のケースだと、相殺が起きないなどトリッキーな動きをする  
相殺は、数値の**大きい方**が適用される  
![スクリーンショット 2021-12-28 14 10 17](https://user-images.githubusercontent.com/78260526/147530324-634c593c-eb18-49dd-9969-037f9eaf80c7.png)  
<img width="235" alt="スクリーンショット 2021-12-28 14 13 17" src="https://user-images.githubusercontent.com/78260526/147530526-0716399e-2ab7-4e8a-b34c-cfc20f15f3fc.png">  
<img width="348" alt="スクリーンショット 2021-12-28 14 14 15" src="https://user-images.githubusercontent.com/78260526/147530534-f8a69752-c852-4dae-9928-f8b079d433c4.png">  

### Molecules/NewsAndEventsMore
<img width="242" alt="スクリーンショット 2021-12-28 15 01 56" src="https://user-images.githubusercontent.com/78260526/147536814-12ee4d5e-50a8-4d6a-8aba-9873df430917.png">  

**inlineについて**  

- `block`: 要素が横までいっぱいに広がり、縦に並んでいく
- `inline`: 要素が平ぺったく横に並んでいく
- `inline-block`: 要素の並び方はinline的で、要素の中身はblock的  

![スクリーンショット 2021-12-28 16 13 51](https://user-images.githubusercontent.com/78260526/147538959-a825544e-59fb-4970-ad97-48199b496d75.png)

### Molecules/Card
<img width="600" alt="スクリーンショット 2021-12-28 17 23 22" src="https://user-images.githubusercontent.com/78260526/147545372-28cf1deb-5345-4fab-83e9-2e38e770215e.png">

#### [transitionについて](https://www.asobou.co.jp/blog/web/css-animation3#transitionanimation)  
- transitionプロパティは:hoverや:activeなど、要素が変化するためのトリガーが必要
- transitionで設定できるプロパティ
  - transition-property: **指定要素の度のプロパティにアニメーションを適用するかを定義する**
  - transition-duration: **アニメーションの時間を定義する**
  - transition-delay: **アニメーションを開始するタイミングを定義する**
  - transition-timing-function: **アニメーション変化の度合を定義する**

### Molecules/Plan
**`<dl><dt><dd>`タグについて**  
- 「description list(dl）= 説明リスト」
- 「description term(dt）= 説明する言葉」
- 「definition / description(dd）= 定義分もしくは説明文」

`「*」`: すべての要素にスタイルを適用する全称セレクタ  
`「>」`: 指定した要素の直下にある要素にスタイルを適用  

**width: auto;とwidth: 100%;の違い**  
1. ブロック要素かインライン要素での違い  

**ブロック要素の場合**  
- width: auto;
  - 横幅　＝　親要素の幅いっぱい
- width: 100%;
  - 横幅　＝　親要素の幅いっぱい  

**インライン要素の場合**  
- width: auto;
  - 横幅　＝　コンテンツ分
- width: 100%;
  - 横幅　＝　コンテンツ分（widthは効かない）  

**インラインブロック要素の場合**  
- width: auto;
  - 横幅　＝　コンテンツ分
- width: 100%;
  - 横幅　＝　親要素の幅いっぱいまで広がる  

[Qiita](https://qiita.com/7note/items/2df14c399f8a233e0328)  

**nth-child**  
`要素:nth-child(値){スタイルの内容}`  
