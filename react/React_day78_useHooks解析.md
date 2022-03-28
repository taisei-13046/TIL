## やったこと

useHooksを解析する

[useHooks](https://usehooks-ts.com/)  


### useEventlistener
[useEventlistener](https://usehooks-ts.com/react-hook/use-event-listener)  

> const refContainer = useRef(initialValue);  
useRef は、.current プロパティが渡された引数 (initialValue) に初期化されているミュータブルな ref オブジェクトを返します。返されるオブジェクトはコンポーネントの存在期間全体にわたって存在し続けます。

useRefは第一引数に関数を渡すこともできる  

#### RefObjectとMutableRefObjectの違いについて

```tsx
interface RefObject<T> {
    readonly current: T | null;
}
```

```ts
interface MutableRefObject<T> {
    current: T;
}
```

useRef() は ref 属性で使うだけではなく、より便利に使えます。これはクラスでインスタンス変数を使うのと同様にして、あらゆる書き換え可能な値を保持しておくのに便利です。

既存の RefObject では新たな用例をサポートできません。DOMの参照をする利用例に対しての定義であって、インスタンス変数のようにユーザーが管理する値であることを前提にしていないからです。hooksのもとで広がった用例に対する表現をサポートするために、 readonly を外す代わりに MutableRefObject が設けられました。

#### インスタンス変数代わりに使うとき
概ね MutableRefObject になるパターンがこちらにあたります。

```ts
// 推論されて、 `MutableRefObject<null>` になる。 `null` だけが入る
useRef(null);
// 推論されて、 `MutableRefObject<undefined>` になる。 `undefined` だけが入る
useRef(undefined);
// 推論されて、 `MutableRefObject<number>` になる。 `number` が入る
useRef(0);
// `MutableRefObject<number | null>` になる。 `number` と `null` が入る
useRef<number | null>(null);
// `MutableRefObject<React.CSSProperties>` になる
useRef<React.CSSProperties>({});
// `MutableRefObject<HTMLElement | null>` になる
useRef<HTMLElement | null>(null);
```

初期値を与えましょう
型引数は初期値からの推論に任せるか、明示的に書くかしましょう

#### DOMやコンポーネントにアクセスする手段として使うとき

```ts
function Comp() {
  const ref = useRef<HTMLDivElement>(null);
  return <div ref={ref}>foo</div>;
}
```

初期値に null を入れましょう
型引数は中身に入りうるDOMノード、またはReactの要素の型だけを与えましょう
初期値が null で型定義に null を含まない場合を条件に、特別に RefObject が得られるようになっています。これを守ることで、上記コード片での ref は `RefObject<HTMLDivElement>` になり、 .current は readonly になります。

