## やったこと
レビューのまとめ

### カスタムフックの返り値どうする？問題
#### 配列で返す場合
一つのコンポーネント内で同じHooksを複数使う場合は命名に柔軟性がありスッキリ書ける。  
しかし、要素数が多く配列の最後の方だけ使いたいといったケースの場合は面倒になる

#### objectで返す場合
Hooksを複数使う場合に命名を変えなくてはいけないため、面倒になる場合も  
しかし、配列のように木構造を気にしなくていいため、単発で使う場合にはすっきりかける

こんな記事もあった  
[custom hookの返り値は配列であるべきか](https://blog.ojisan.io/why-hooks-need-array/)  
[Should hooks always return an array?](https://dev.to/theianjones/should-hooks-always-return-an-array--21np)  

#### 特定の値だけを抽出するときに不便
```ts
export const useHoge = () => {
  // something
  return [loading, data, error, refetch];
};
```
この場合にdataだけが欲しいとなると、アクセスができない  

```ts
const getVal = () => [1, 2, 3, 4];

const [_, second] = getVal();
```
```ts
> second
2
```

 一応この形で抽出することはできる  
 
 ### Writing Your Own React Hooks, the Return Value
















