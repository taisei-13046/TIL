## やったこと
hackasonの準備！！

### 型の拡張
```ts
type TPerson = {
  name: string;
  old: number;
};

type TPerson2 = TPerson & {
  address: string;
};
```
型を拡張する方法は`&`でつなぐ  

### リロードしてもデータを保存する
[リロードしてもデータを保持するためには「localStorage」](https://qiita.com/Ryusou/items/8bce84e7b036114b8d72)  
リロードしてもデータを保存するためには**localStorage**を使用すれば解決できる  
使い方は超簡単!  
```js
localStorage.setItem('myCat', 'Tom');

var cat = localStorage.getItem("myCat");
console.log(cat)
//出力結果　> "Tom"
```
