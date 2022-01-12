## やったこと
frontendの最終課題とハッカソンの準備

### box-sizingでborderを内側に
box-sizingプロパティはpaddingとborderの値を幅、高さに含めるかどうかのプロパティ  

指定する3つの値  
- box-sizing: content-box; ←初期値！
 - paddingとborderを幅と高さに含めない
- box-sizing: border-box;
 - paddingとborderを幅と高さに含める
- box-sizing: inherit;
 - 親要素のborder-boxの値を引き継ぐ

### useStateの値と関数をpropsに渡す
```ts
type PropsType = {
	onClickId: number | undefined;
	setOnClickId: React.Dispatch<React.SetStateAction<number | undefined>>
}
```
そこで感じた。  
`React.Dispatch<React.SetStateAction<number | undefined>>`これ何？？？  

React.Dispatchとは。React Hooksでの型定義は以下のようになっている
```ts
type Dispatch<A> = (value: A) => void;
```
つまり、**「Aという型を指定して戻り値はない」という意味の関数**  

React.SetStateActionとは,
```ts
type SetStateAction<S> = S | ((prevState: S) => S);
```
**「何かしらのデータそのもの」もしくは「受け付けた引数の型を返す何かしらの関数」である**  

なので、このようにもかけますし、
```ts
  const increment = () => {
    setValue(value + 1);
  }
```
```ts
  const increment = () => {
    setValue((value) => value + 1);
  }
```
このように関数を渡すことも可能です。

参考資料  
[【React】TypeScriptで用意されている型一覧](http://www.code-magagine.com/?p=13261)  
