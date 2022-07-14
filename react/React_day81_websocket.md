## やったこと

websoketについてまとめる
[WebSocket Protocol](https://triple-underscore.github.io/RFC6455-ja.html)  

#### 背景
歴史的に、 クライアント↔サーバ間の双方向通信を必要とする web アプリ （例：インスタントメッセンジャや, ゲーム用アプリ） の作成においては、 サーバから更新をポーリングするために，上流への通知を別個な HTTP コールとして送信する間、 HTTP の濫用を要していた  

#### プロトコルの概観
このプロトコルは、 2 つのパート — ハンドシェイクとデータ転送 — からなる  

ハンドシェイクの成功後、 クライアントとサーバは，この仕様において “メッセージ” と呼ばれる，概念的な単位を成すデータを 相互に転送しあう。  

#### opening ハンドシェイク

単独のポートを，サーバとやりとりする HTTP クライアントからも, およびサーバとやりとりする WebSocket クライアントからも利用できるようにするため、 opening ハンドシェイクは，HTTP に基づくサーバ側ソフトウェアや中継点と互換になることが意図されている。

```
GET /chat HTTP/1.1
Host: server.example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Origin: http://example.com
Sec-WebSocket-Protocol: chat, superchat
Sec-WebSocket-Version: 13
```

サーバからのハンドシェイクは、 クライアントからのハンドシェイクよりもずっと単純になる。

```
HTTP/1.1 101 Switching Protocols
```

101 以外のどの応答コードも WebSocket ハンドシェイクが完了しておらず，HTTP の意味論が依然として適用されることを指示する。

#### closing ハンドシェイク
接続の close を指示する制御フレームを
  - 送信した端点は、それ以降 データを送信しない。
  - 受信した端点は、それ以降に受信したデータを破棄する。

#### 設計理念

WebSocket Protocol は、 フレーム法は必要最小限に留めるべき，とする原則に基づいて設計されている   

概念的には，WebSocket は、 ちょうど，次だけを行う TCP の一つ上の層である：
- ブラウザ用の，web 生成元に基づくセキュリティモデルを追加する
- 1 個のポート上における複数のサービスを, および 1 個の IP アドレス上における複数のホスト名をサポートするための、 アドレス法とプロトコル命名の仕組みを追加する
- TCP の一つ上にフレーム法の仕組みを積層することにより、 TCP の下層の IP パケットの仕組みを（長さ制限を取り払いつつ）取り戻す
- プロキシや他の中継点が介在していても働くように設計された、 追加の closing ハンドシェイクを帯域内に含む











