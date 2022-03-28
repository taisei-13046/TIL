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


#### WindowEventMap
[React HooksでWindowEventMapを使う](https://zenn.dev/tera_ny/articles/246b1d9024ff6a)  

```tsx
const useWindowEvent = <K extends keyof WindowEventMap>(
  type: K,
  listener: (this: Window, ev: WindowEventMap[K]) => any,
) =>
{
  window.addEventListener(type, listener);
}
```

`<K extends keyof WindowEventMap>`でWindowEventMapのkeyをunion型でもった型を継承できる  

### useInterval

[setInterval関数](https://developer.mozilla.org/ja/docs/Web/API/setInterval)  

setInterval() メソッドは Window および Worker メソッドで提供され、一定の遅延間隔を置いて関数やコードスニペットを繰り返し呼び出します。

このメソッド、インターバルを一意に識別するインターバル ID を返します。よって clearInterval() を呼び出して、後でインターバルを削除できます。   

```js
var intervalID = setInterval(func, [delay, arg1, arg2, ...]);
var intervalID = setInterval(function[, delay]);
var intervalID = setInterval(code, [delay]);
```

**引数**  
**func**  
delay ミリ秒が経過するたびに実行する関数です。最初の実行は delay ミリ秒後に行われます。

**code**  
関数の代わりに文字列を含める構文も許容されており、 delay ミリ秒が経過するたびに文字列をコンパイルして実行します。 eval() の使用にリスクがあるのと同じ理由で、この構文は推奨しません。

**delay省略可**  
指定した関数またはコードを実行する前にタイマーが待つべき時間をミリ秒 (1/1000 秒) 単位で指定します。引数が 10 より小さい場合は、10 を使用します。実際の遅延が長くなることがあります。例えば 遅延が指定値より長い理由 in setTimeout() をご覧ください。

**返値**  
返値 intervalID は 0 ではない正の整数値で、 setInterval() を呼び出して作成したタイマーを識別します。この値を clearInterval() へ渡せば、インターバルを取り消すことができます。

setInterval() と setTimeout() は同じ ID プールを共有しており、 clearInterval() と clearTimeout() は技術的に入れ替えて使用できることを意識すると役に立つでしょう。ただし明快さのために、コードを整備するときは混乱を避けるため、常に一致させるようにするべきです。

### useDebounce 

[useDebounce](https://usehooks-ts.com/react-hook/use-debounce)  














