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














