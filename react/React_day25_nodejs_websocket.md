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

その他、`io.in(roomId).emit`でdataを送信できる  
`socket.leave(roomId);`ではroomIdから退出する  

## useRefでundefinedのエラーが起きまくった。。。
### useRefとは
```ts
const refContainer = useRef(initialValue);
```
useRef は、.current プロパティが渡された引数 (initialValue) に初期化されているミュータブルな ref オブジェクトを返す  
返されるオブジェクトはコンポーネントの存在期間全体にわたって存在し続ける  
本質的に useRef とは、書き換え可能な値を .current プロパティ内に保持することができる「箱」のようなもの  
documentの例  
```jsx
function TextInputWithFocusButton() {
  const inputEl = useRef(null);
  const onButtonClick = () => {
    // `current` points to the mounted text input element
    inputEl.current.focus();
  };
  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  );
}
```
`<div ref={myRef} />` のようにして React に ref オブジェクトを渡した場合、React は DOM ノードに変更があるたびに .current プロパティをその DOM ノードに設定する  
useRef() を使うことと自分で {current: ...} というオブジェクトを作成することとの唯一の違いとは、useRef は毎回のレンダーで同じ ref オブジェクトを返す、ということ  
useRef は中身が変更になってもそのことを通知しない  
.current プロパティを書き換えても再レンダーは発生しない  
公式doc [useRef](https://ja.reactjs.org/docs/hooks-reference.html#useref)  
Qiita記事 [React hooksを基礎から理解する (useRef編)](https://qiita.com/seira/items/0e6a2d835f1afb50544d)  

再レンダリングはしたくないけど、内部に保持している値だけを更新したい場合はuseRefを使うのが良い  

### そして本題。useRefの型定義でエラーが起きている
<img width="567" alt="スクリーンショット 2022-01-20 13 45 21" src="https://user-images.githubusercontent.com/78260526/150275168-8ffc926f-d77f-485d-b9a3-9aab37c6d1d1.png">  

解決方法は二つ。  
まずはuserRefに型指定が必要だった。  
```ts
const socketRef = useRef<Socket<DefaultEventsMap> | null>();
```
二つ目、
```ts
if (socketRef.current !== undefined && socketRef.current !== null) {
       socketRef.current.disconnect();
}
```
TSの型ガードをする必要があった。  
[型ガード](https://typescript-jp.gitbook.io/deep-dive/type-system/typeguard)  


### [React + TypeScript: useRefの3つの型指定と初期値の使い方](https://qiita.com/FumioNonaka/items/feb2fd5b362f2558acfa)  
```ts
const nullRef = useRef<number>(null);
const nonNullRef = useRef<number>(null!);
const nullableRef = useRef<number | null>(null);
```
#### 1. nullで初期化したとき
useRefフックの初期値にnullを与えると、戻り値のrefオブジェクトは読み取り専用になる  
つまり、currentプロパティは書き替えられない  

#### 2. 初期値nullに非nullアサーション演算子!を添える
初期値のnullに非nullアサーション演算子(non-null assertion operator)!を添えてnull!とすると、今度はrefオブジェクトのcurrentプロパティが書き替えられる  
代入できるのは指定した型のみで、nullは入れられない  

### 3. 型指定にnullを含める
useRefフックに書き替え可能の初期値nullを渡したうえで、あえてnullも代入値に含めたいときは、型指定にユニオン型(union type)|で加える  
```ts
// 読み取り専用。
const nullRef = useRef<number>(null);
// 指定した型の値で書き替えられる。nullは不可。
const nonNullRef = useRef<number>(null!);
// 型指定にnullを含める。
const nullableRef = useRef<number | null>(null);
```



