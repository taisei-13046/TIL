## やったこと
フロントの最終課題とハッカソンの準備  

### display: inline-flexって何？？
Molecules/NewsAndEventsMoreにおいて、`a`タグの範囲がdisplay:flexにすると横全体になってしまう現象が起きた。  
それを解決するのがdisplay: inline-flexであったが、なぜそれで解決されるのかわからなかった。  
`display: inline-flex`とは、要素をインラインレベルのflexコンテナとして定義する  
とあるがそのまますぎる。  

`display: inline-flex`は`display: inline-box`の拡張版(flexの要素も持つ)  
とあったが、改めてinline-boxの理解が甘い気がした  

<img width="733" alt="スクリーンショット 2022-01-16 23 29 51" src="https://user-images.githubusercontent.com/78260526/149664147-df72c7dd-9a9e-4668-ae27-81546af1fb37.png">

inline-block要素の特徴
- 横幅と高さが指定できる
- ブロック要素と同じ余白のつき方をする
- 横幅の初期値が内容で決まる
- 前後の要素と横に並ぶ

このうちの**横幅の初期値が内容で決まる**が今回のポイントだった。  
