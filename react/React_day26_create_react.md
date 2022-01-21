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

    socketRef.current.on(NEW_CHAT_MESSAGE_EVENT, (message: any) => {
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

  const sendMessage = useCallback((messageBody: any) => {
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

### 2. messageデータを受け取って、さらに最新のメッセージを追加して返す
```ts
  if (socketRef.current !== undefined && socketRef.current !== null) {
    const incomingMessage = {
      ...message,
      owndByCurrentUser: message.senderId === socketRef.current.id,
    };
    setMessages((messages) => [...messages, incomingMessage]);
  }
});
```

### 3. useEffectのコールバック関数でdisconnect処理
```ts
return () => {
  if (socketRef.current !== undefined && socketRef.current !== null) {
    socketRef.current.disconnect();
  }
};
```
