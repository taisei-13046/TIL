## やったこと
案件作業を進めた

### SVGについて
意外に今まで知らなかったSVG  
それぞれの渡せる要素についてまとめてみた  
まあ読むのはお決まりのMDNですよね！   
[SVG: Scalable Vector Graphics](https://developer.mozilla.org/ja/docs/Web/SVG)  

Scalable Vector Graphics (SVG) は、二次元ベースのベクター形式のための XML に基づくマークアップ言語です。  
旧来の JPEG や PNG のようなビットマップ画像形式と比較して、 SVG 形式のベクター画像は、品質を損なうことなく任意の大きさでレンダリングすることができ、テキストを更新することで、グラフィックエディターを使用せずに簡単にローカライズすることができます。  

案件で使用されていた要素を調べた  

#### ViewBox
[SVGのviewBoxをわかりやすく紐解く](https://www.indetail.co.jp/blog/13002/)  
viewBoxを指定することで、描画エリアのアスペクト比、およびその中の要素の相対的なサイズを決定します。  
`viewBox="x, y, width, height"`  

#### fill-rule
fill-rule 属性は複数のパスで囲まれる部分がパスの内側かどうかを判断して塗りつぶしを制御するための属性で、値としては主に nonzero (省略した場合のデフォルト値) と evenodd があります。  
`fill-rule="nonzero" (省略時のデフォルト値)`  

#### clip-rule
`<clipPath>`要素内に含まれるgraphics要素にのみ適用されます。clip-rule属性は、基本的にfill-rule属性と同じ働きをしますが、`<clipPath>`定義に適用される点が異なります。  

#### fill
fill 属性には使われ方により2つの意味があります.  1つは図形やテキストに使われた場合で，その要素を塗りつぶす色を意味します．もう1つはアニメーションに使われた場合で，そのアニメーションの最終状態を定義します．  

#### stroke
stroke属性は与えられた図形要素の外側に描画される色を定義します。stroke属性のデフォルト値は none です.  

#### preserveAspectRatio
preserveAspectRatio 属性は、与えられたアスペクト比を提供するビューボックスを持つ要素が、異なるアスペクト比を持つビューポートにどのように適合しなければならないかを示すものである。  
svgタグには、何も指定しなくてもデフォルトで、`preserveAspectRatio="xMidYMid meet"`が指定されています。




参考資料 [SVGの記述方法](https://qiita.com/takeshisakuma/items/777e3cb0a54ea7b1dbe7)  










