## やったこと
リアルタイムチャット機能を追加した

### react-router-domにおけるparamsの受け渡し
以前までは(v5)propsにparamsの値が渡されるようになっていたが、v6からparamsの取得には`useParams`の使用が必須になった  

## React側でのsocket.ioの設定
今回はChat機能を作るためにuseChatというカスタムフックを作成する  

```ts
import { useCallback, useEffect, useRef, useState } from "react";
import socketIOClient, { Socket } from "socket.io-client";
import { DefaultEventsMap } from "socket.io/dist/typed-events";

const NEW_CHAT_MESSAGE_EVENT = "newChatMessage";
const SOCKET_SERVER_URL = "http://localhost:4000";

type MessageType = {
  ownedByCurrentUser: "my-message" | "received-message";
  body: string | null;
};

export const useChat = (roomId: number) => {
  const [messages, setMessages] = useState<MessageType[]>([]);
  const socketRef = useRef<Socket<DefaultEventsMap> | null>();

  useEffect(() => {
    socketRef.current = socketIOClient(SOCKET_SERVER_URL, {
      query: { roomId },
    });

    socketRef.current.on(NEW_CHAT_MESSAGE_EVENT, (message) => {
      if (socketRef.current !== undefined && socketRef.current !== null) {
        const incomingMessage = {
          ...message,
          owndByCurrentUser: message.senderId === socketRef.current.id,
        };
        setMessages((messages) => [...messages, incomingMessage]);
      }
    });

    return () => {
      if (socketRef.current !== undefined && socketRef.current !== null) {
        socketRef.current.disconnect();
      }
    };
  }, [roomId]);

  const sendMessage = useCallback((messageBody) => {
    if (socketRef.current !== undefined && socketRef.current !== null) {
      socketRef.current.emit(NEW_CHAT_MESSAGE_EVENT, {
        body: messageBody,
        senderId: socketRef.current.id,
      });
    }
  }, []);

  return { messages, sendMessage };
};
```
では上から見ていく  

### 1. queryをセットして、エンドポイントにアクセスする
```ts
socketRef.current = socketIOClient(SOCKET_SERVER_URL, {
  query: { roomId },
});
```

### 2. messageデータを受け取って、さらに最新のメッセージを追加して返すイベントハンドラを登録
```ts
socketRef.current.on(NEW_CHAT_MESSAGE_EVENT, (message: any) => {
  if (socketRef.current !== undefined && socketRef.current !== null) {
    const incomingMessage = {
      ...message,
      owndByCurrentUser: message.senderId === socketRef.current.id,
    };
    setMessages((messages) => [...messages, incomingMessage]);
  }
});
```

### 3. useEffectのクリーンアップ関数でdisconnect処理
```ts
return () => {
  if (socketRef.current !== undefined && socketRef.current !== null) {
    socketRef.current.disconnect();
  }
};
```
useEffect()では、副作用関数がクリーンアップ関数を返すことで、マウント時に実行した処理をアンマウント時に解除する  
またその副作用関数は、毎回のレンダリング時に実行され、新しい副作用関数を実行する前に、ひとつ前の副作用処理をクリーンアップする  [

### 4. それらを実行する関数と値を返す
```ts
const sendMessage = useCallback((messageBody: any) => {
  if (socketRef.current !== undefined && socketRef.current !== null) {
    socketRef.current.emit(NEW_CHAT_MESSAGE_EVENT, {
      body: messageBody,
      senderId: socketRef.current.id,
    });
  }
}, []);

return { messages, sendMessage };
```

