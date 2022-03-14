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




