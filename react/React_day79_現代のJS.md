## やったこと
現代のJSを読む  

[現代の JavaScript チュートリアル](https://ja.javascript.info/)  


## ブラウザ編

### ブラウザイベントの紹介
イベント は何かが起きたと言う信号です。すべての DOM ノードはこのような信号を生成します(ただし、イベントは DOM に限ったものではありません)。  

#### イベントハンドラ
イベントに反応するために、**ハンドラ – イベント発生時に実行する関数 – **を割り当てることができます。

ハンドラは、ユーザのアクション時に JavaScript コードを実行する方法です。

ハンドラを割り当てる方法はいくつかあります。最も簡単なものから始め、それらを見ていきましょう。

ハンドラってイベント発生時に実行する関数だったのか。。。知らなかった


#### HTML 属性
ハンドラは `on<event>` と言う名前の属性で、 HTML 上で設定できます。

例えば、input に対して click ハンドラを割り当てるためには、このように onclick を使います:

```js
<input value="Click me" onclick="alert('Click!')" type="button">
```

#### addEventListener
ハンドラを割り当てるための前述の根本的な問題は – １つのイベントに複数のハンドラを割り当てられないことです。  

```js
input.onclick = function() { alert(1); }
// ...
input.onclick = function() { alert(2); } // 前のハンドラを上書きします
```

Web標準の開発者はずっと前に理解しており、特別なメソッド addEventListener と removeEventListener を使うことで、ハンドラを管理する別の方法を提案しました。これらはこのような問題から解放されています。

ハンドラを追加する構文は次のようになります:  

```js
element.addEventListener(event, handler[, phase]);
```

##### event
イベント名, e.g. "click".

##### handler
ハンドラ関数.

##### phase
オプションの引数で、ハンドラが動作する “フェーズ” です。後ほど説明します。通常は使いません。
ハンドラを削除する場合は removeEventListener を使います:

```js
// addEventListener とまったく同じ引数です
element.removeEventListener(event, handler[, phase]);
```

#### イベントオブジェクト
イベントを適切に処理するためには、私たちは何が起こったのかをもっと知りたいです。単に “click” や “keypress” だけではなく、ポインタの座標はなにか？どのキーが押されたのか？などです。

イベントが起こったとき、ブラウザは イベントオブジェクト を作り、そこに詳細を入れ、ハンドラの引数として渡します。  

#### オブジェクトハンドラ: handleEvent
addEventListener を使用したイベントハンドラとしてオブジェクトを割り当てることも可能です。イベントが発生するとき、その handleEvent メソッドが呼ばれます。  

```js
<button id="elem">Click me</button>

<script>
  elem.addEventListener('click', {
    handleEvent(event) {
      alert(event.type + " at " + event.currentTarget);
    }
  });
</script>
```

言い換えると、addEventListener がハンドラとしてオブジェクトを受け取ると、イベント時に object.handleEvent(event) を呼び出します。  

### バブリング と キャプチャリング
#### バブリング(Bubbling)
バブリングの原理はシンプルです。

要素上でイベントが起きると、最初にその上のハンドラが実行され、次にその親のハンドラが実行され、他の祖先に到達するまでそれらが行われます。  

#### event.target
親要素のハンドラは、常に実際に発生した場所についての詳細を取得できます。

イベントを起こした最も深くネストされた要素は ターゲット 要素と呼ばれ、 event.target でアクセス可能です。

this (=event.currentTarget) との違いは次です:

event.target – はイベントを始めた “ターゲット” 要素で、バブリングプロセスを通して変化しません。
this – は “現在” の要素で、現在実行中のハンドラを持ちます。
例えば、単一のハンドラ form.onclick を持っている場合、その form 中のすべてのクリックを “キャッチ” できます。どこでクリックされたかは関係なく、`<form>` までバブルし、そのハンドラを実行します。

form.onclick ハンドラの中は:

this (=event.currentTarget) は `<form>` 要素です。なぜなら、ハンドラはそこで動いているからです。
event.target は実際にクリックされた form の内側にある具体的な要素です。


#### キャプチャリング(Capturing)
標準 DOM Events はイベント伝搬の３つのフェーズを説明しています。:

キャプチャリングフェーズ – イベントが要素へ下りていきます。
ターゲットフェーズ – イベントがターゲット要素に到達しました。
バブリングフェーズ – イベントが要素から上にバブルします。

<img width="679" alt="スクリーンショット 2022-03-28 19 36 16" src="https://user-images.githubusercontent.com/78260526/160380589-7bafd3d9-43ec-476a-95e0-1a0bbcc4b6f5.png">

`on<event>`プロパティまたは HTML属性、もしくは addEventListener(event, handler) を使って追加されたハンドラはキャプチャリングについて何も知りません。それらはフェーズ 2 と 3 でのみ実行されます。

キャプチャリングフェーズでイベントをキャッチするには、addEventListener の３つ目の引数を true にする必要があります。

最後の引数は２つのとり得る値があります:

false (デフォルト) の場合、ハンドラはバブリングフェーズで設定されます。
true の場合、ハンドラはキャプチャリングフェーズで設定されます。

#### イベント移譲(Event delegation)










