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




