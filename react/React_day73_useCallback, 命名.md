## やったこと
レビューのまとめ

### onClick属性に渡す関数をmemo化したかった。
自分は最初、以下のようにコードを記述した。  

```tsx
const onClickTag = useCallback(() => {
    ~~~~~
  },
  []
)

// map内での関数
onClick={() => onClickTag(props.id)}
```

しかし、これでは毎回アロー関数が生成されてしまい、useCallbackでmemo化した意味がなかった。  
そのため、onClick属性には以下のように関数を渡す必要がある。  

```tsx
onClick={onClickTag}
```

これであれば、memo化の恩恵を受けることができる  

ではなぜ、上記の記述をしてしまったのか？  
A. それは、引数を持った関数をuseCallbackでmemo化したかったためだ  

```tsx
onClick={onClickTag(props.id)}
```

こういった記述はそもそもできない。  

**これは、onClick属性に対して関数を渡す必要があるのに、関数の結果を渡してしまっているからだ**  

この形式でエラーを起こさないようにするためには、

```ts
const handleChange = useCallback((id) => () => {
  doStuff(id)
, []}

onClick={handleChange(id)}
```

上記のようにカリー化をする必要があるが、memo化をすることはできない。

そのため、memo化できるところとそうでないところはしっかり判断していく必要がある  

### 変数の命名について
自分は最初, propsの完了ボタンが押された際に渡す関数の命名に  
`onClickCompleteButton`  
という命名をしていた。。

これはこのコンポーネント内部から見ると違和感を感じないかもしれない。  

しかし、コンポーネントを使う側からの視点で見ると、CompleteButtonがClickされた時という情報が必要かどうかでいうとそうではない。  

**関心の分離**  

そのため、ここでは`onComplete`という「何かしらが完了した時」という情報にとどめておいた方がいい。  












