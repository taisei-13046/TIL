## やったこと
ハッカソンのアプリにwebsocketでチャット機能を追加した  
node.jsは初めて触るため、何をしていいかわからない😅

## サーバ環境を構築する
参考にした資料[Build a Real-Time Chat App With React Hooks and Socket.io](https://medium.com/swlh/build-a-real-time-chat-app-with-react-hooks-and-socket-io-4859c9afecb0)  

初めにserver用のフォルダを作成して、初期化する  
```
server - index.js
       - package.json
```
package.jsonはそのディレクトリ下で`npm init`コマンドを実行することで作成できる  

```node
const server = require("http").createServer();
```
初めにcreateServerでserverのオブジェクトを作成する  

### 一度node.jsの基本を抑えることにする
[Node.jsのスクリプトの基本を覚えよう (1/5)](https://www.tuyano.com/index3?id=1126003)  

```node
var http = require('http');
```
おおむね`import`と同じ意味を持つ  
引数に、読み込むオブジェクト名を指定することで、そのオブジェクトが読み込まれる  

```node
var server = http.createServer();
```
serverオブジェクトを作成する  
httpオブジェクトの「createServer」メソッドを呼び出してhttp.Serverオブジェクトを作成する  
このオブジェクトを用意し、必要な設定をしてからサーバーとして実行する  

```node
server.on('request', doRequest);
```
リクエスト処理を設定する  
on」というメソッドは、指定のイベント処理を組み込むためのもので、第一引数にイベント名を、第２引数に組み込む処理（関数）をそれぞれ指定する  

```node
server.listen(1234);
```
http.Serverオブジェクトの準備が整ったら、「listen」メソッドを実行する  
これにより、サーバーは待ち受け状態となり、クライアントからリクエストがあればそれを受け取り処理するようになる  

ではチャットアプリ作成に戻る  

```node
const io = require("socket.io")(server, {
  cors: {
    origin: "*",
  },
});
```
corsの設定を踏まえた上でsocket.ioをimportする  

```node
io.on("connection", (socket) => {
  console.log(`Client ${socket.id} connected`);

  const { roomId } = socket.handsake.query;
  socket.join(roomId);

  socket.on(NEW_CHAT_MESSAGE_EVENT, (data) => {
    io.in(roomId).emit(NEW_CHAT_MESSAGE_EVENT, data);
  });

  socket.on("disconnect", () => {
    console.log(`Client ${socket.id} disconnencted`);
    socket.leave(roomId);
  });
});
```
リクエストの処理を記述していく  

```node
io.on("connection", (socket) => {
});
```
[Socket.io のよく使う関数とか](https://qiita.com/uranesu/items/8ee0dbe4e472f9fffa49)  

今回は深掘りするとどこまでもできてしまいそうなので、浅く理解する  
`io.on`をするとイベントハンドラを登録することができる  
```node
/*connection(webSocket確立時)イベント時
クライアントが接続するたびにイベントハンドラを登録*/
io.on('connection', socket => {
    socket.on('event', () => {
    });
});
```

`const { roomId } = socket.handshake.query;`でroomIdを取得  

そのroomIdを使用して`socket.join(roomId);`することで、`roomId`に接続することができる  

