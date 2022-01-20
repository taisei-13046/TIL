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
初めにcreateServerでserverのインスタンスを作成する  

### 一度node.jsの基本を抑えることにする
[Node.jsのスクリプトの基本を覚えよう (1/5)](https://www.tuyano.com/index3?id=1126003)  


