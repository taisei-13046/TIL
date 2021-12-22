# やったこと
### パフォーマンスの最適化について (useMemo, useCallback, React.memo)

メモ化とは  
*関数内における任意のサブルーチンを呼び出した結果を後で再利用するために保持しておき、  
その関数が呼び出されるたびに再計算されることを防ぐ、プログラム高速化の手法のこと*

<br/>

useMemo: **関数の結果を保持する**

```
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```
- メモ化された値を返す
- 依存配列の要素のいずれかが変化した場合にのみメモ化された値を再計算する
- **渡した関数はレンダー中に実行される**
- 配列が渡されなかった場合は、新しい値がレンダーごとに毎回計算される
- パフォーマンス最適化のために使うものであり、意味上の保証があるものだと考えない

> 将来的に React は、例えば画面外のコンポーネント用のメモリを解放するため、などの理由で、メモ化された値を「忘れる」ようにする可能性があります。useMemo なしでも動作するコードを書き、パフォーマンス最適化のために useMemo を加えるようにしましょう。

<br />

useCallback: **関数自体をメモ化する**
```
const memoizedCallback = useCallback(
  () => {
    doSomething(a, b);
  },
  [a, b],
);
```
- メモ化されたコールバックを返す
- 関数に特化している
- インラインのコールバックとそれが依存している値の配列を渡す
- 不必要なレンダーを避けるために（例えば shouldComponentUpdate などを使って）参照の同一性を見るよう最適化されたコンポーネントにコールバックを渡す場合に便利
- **useCallback(fn, deps) は useMemo(() => fn, deps) と等価**

useCallbackなどはとにかく使った方がいい!
> 特にカスタムフックが関数を返すなら常にuseCallbackで囲む [参照](https://blog.uhy.ooo/entry/2021-02-23/usecallback-custom-hooks/)
<br />

React.memo
```
const MyComponent = React.memo(function MyComponent(props) {
  /* render using props */
});
```
> もしあるコンポーネントが同じ props を与えられたときに同じ結果をレンダーするなら、結果を記憶してパフォーマンスを向上させるためにそれを React.memo でラップすることができます。つまり、React はコンポーネントのレンダーをスキップし、最後のレンダー結果を再利用します。

##### その他やったこと
- DDDを読む
- Djagno(最終課題)修正
- 「りあクト！」を読む

### 次回
今回memo化を調べる文脈でカスタムフックの話がたびたび出てきた。  
useCallbackとの関連性も含めて調べてみたい。
