## やったこと
reactのmapでkeyにindexを渡す危険性について

### Index as a key is an anti-pattern

[Index as a key is an anti-pattern](https://robinpokorny.medium.com/index-as-a-key-is-an-anti-pattern-e0349aece318)  

このサイトのdemoがめちゃくちゃわかりやすい！  
[DEMO](https://jsbin.com/wohima/edit?output)  

#### keyにindexを渡してもいい条件
1. 配列とその中の要素が静的(計算も変更もされない)
2. 配列の中の要素がidを持ってない
3. 配列がreorderやfilterされることが絶対ない

### useMemoとuseCallbackの違い
[useCallback vs useMemo](https://medium.com/@jan.hesters/usecallback-vs-usememo-c23ad1dc60)  

The React docs say that useCallback:

> Returns a memoized callback.

And that useMemo:

> Returns a memoized value.

In other words, useCallback gives you **referential equality between renders for functions**. And useMemo gives you **referential equality between renders for values**.    

useCallbackもuseMemoもどちらもreferential equalityで評価される。  

Since JavaScript has first-class functions, useCallback(fn, deps) is equivalent to useMemo(() => fn, deps).  

JSは第一級関数なので
```
useCallback(fn, deps)
```
```
useMemo(() => fn, deps)
```
は等価である  

So what is the difference? 

useCallback returns **its function uncalled so you can call it later**, while useMemo calls **its function and returns the result**.    

```js

function foo() {
  return 'bar';
}

const memoizedCallback = useCallback(foo, []);
const memoizedResult = useMemo(foo, []);

memoizedCallback;
// ƒ foo() {
//   return 'bar';
// }
memoizedResult; // 'bar'
memoizedCallback(); // 'bar'
memoizedResult(); // 🔴 TypeError
```

```tsx
const fetchUser = async () => {
  const res = await fetch(
    `https://jsonplaceholder.typicode.com/users/${userId}`
  );
  const newUser = await res.json();
  setUser(newUser); // 🔴 setState triggers re-render
};

useEffect(() => {
  fetchUser();
}, [fetchUser]); // fetchUser is a new function on every render
```
上記のようなコードがあったときにこれでは、毎回useEffectが実行されてしまう。

理由としては、毎回fetchUser関数が新しい参照先で生成されるから。  



