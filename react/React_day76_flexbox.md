## やったこと

## Flex-boxについて
もう一回MDNを読もう

[フレックスボックスの基本概念](https://developer.mozilla.org/ja/docs/Web/CSS/CSS_Flexible_Box_Layout/Basic_Concepts_of_Flexbox)  

### フレックスボックス 2 つの軸

フレックスボックスを使うときは 2 つの軸、つまり**主軸 (main axis)** と**交差軸 (cross axis) **の観点から考える必要があります。主軸は flex-direction プロパティによって定義され、交差軸は主軸に垂直に交わる軸です。  

主軸は flex-direction によって定義され、4 種類の値をとることができます。

- row
- row-reverse
- column
- column-reverse

#### アクセシビリティの考慮
flex-direction プロパティを row-reverse または column-reverse の値で使うと、視覚上の表示と DOM の順序が一致しなくなります。これは、画面リーダーなどの支援技術を使っている視覚障害者に不利な影響を及ぼします。視覚的な (CSS の) 順序が重要である場合は、画面リーダーの利用者は正しい読み上げ順序でアクセスすることができなくなります。  

### 行の先頭と末尾
理解が必要なもう一つの重要事項は、フレックスボックスは文書の書字方向を仮定しないという点です。 CSS は過去には、左から右への横書きの書字方向に過度に偏っていました。最近のレイアウト方法は多様な書字方向に対応しており、したがってテキスト行が左上から始まり右に進み、新しい行が下に続くということを仮定しません。  

もし flex-direction が row で言語が英語の場合、主軸の先頭は左で末尾は右になります。

一方で言語がもしアラビア語であった場合、主軸の先頭は右で末尾が左になります。

### フレックスコンテナー
フレックスボックスを使ってレイアウトされる文書の領域は、フレックスコンテナーと呼ばれています。フレックスコンテナーを作るには、その領域のコンテナーに対して display プロパティの値を flex もしくは inline-flex に設定します。またこれにより、このコンテナー直下の子要素がフレックスアイテムとなります。

### flex-flow 一括指定プロパティ
flex-direction と flex-wrap の 2 つのプロパティは、flex-flow 一括指定プロパティにより 2 つ同時に指定することができます。最初に指定される値が flex-direction で、2 つ目の値が flex-wrap です。  

1 つ目の値を flex-direction に使える値 (row, row-reverse, column, column-reverse のいずれか)   

2 つ目の値を wrap か nowrap 

### フレックスアイテムに適用されるプロパティ
フレックスアイテムに対してさらなる制御をするために、アイテムを直接操作対象にすることができます。以下の 3 つのプロパティを使用します。

- flex-grow
- flex-shrink
- flex-basis

### 主軸に沿ったフレックスアイテムの比率の制御
このガイドでは、フレックスアイテムに適用され、主軸に沿ってアイテムの寸法と自由度を制御することができる三つのプロパティ — flex-grow, flex-shrink, flex-basis を見ていきます。  

3 つのプロパティは、フレックスアイテムの自由度を以下の観点から制御します。

- flex-grow: このアイテムが得る正の自由空間はどれくらいか。
- flex-shrink: このアイテムから縮小できる負の自由空間はどれくらいか。
- flex-basis: 伸長や縮小が発生する前のアイテムの寸法はいくつか。

フレックスアイテムをレイアウトするための空間を確保するには、まずアイテムの大きさをブラウザーが知る必要があります。

CSS には min-content と max-content という概念があります。これらのキーワードは CSS Intrinsic and Extrinsic Sizing 仕様書で定義されており、長さの単位の代わりに使用することができます。

#### max-contentとmin-content
widthにmax-content, min-contentを指定するとどうなるのか？

width: max-content; を指定すると、テキストが収まる最小幅がwidthになる  

<img width="1031" alt="スクリーンショット 2022-03-27 10 30 55" src="https://user-images.githubusercontent.com/78260526/160262839-3bf63a4e-149f-4c80-9871-7a75a4e9b775.png">

min-contentを指定したブロックは、テキストを単語で区切り、最大幅となる「sample-3」にあわせたwidthに調整されます。


### 正と負の自由空間









