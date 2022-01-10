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
