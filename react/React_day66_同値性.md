## やったこと
React(JS)の同値性について  
またそこからmemo化の理解を深める  

## How to Compare Objects in JavaScript
[How to Compare Objects in JavaScript](https://dmitripavlutin.com/how-to-compare-objects-in-javascript/)  

### 1. Referential equality
JSには3つの比較演算子がある  
- The strict equality operator **`===`**
- The loose equality operator **`==`**
- **`Object.is()`** function

When comparing objects using any of the above, the comparison evaluates to true **only if the compared values reference the same object instance.** This is the referential equality.  

重要: only if the compared values reference the same object instance.  
同じobjectのインスタンスを返した場合でないとtrueにはならない

```js
const hero1 = {
  name: 'Batman'
};
const hero2 = {
  name: 'Batman'
};
hero1 === hero1; // => true
hero1 === hero2; // => false
hero1 == hero1; // => true
hero1 == hero2; // => false
Object.is(hero1, hero1); // => true
Object.is(hero1, hero2); // => false
```

### 2. Manual comparison
The obvious way to compare objects by content is to read the properties and compare them manually.

```js
function isHeroEqual(object1, object2) {
  return object1.name === object2.name;
}
const hero1 = {
  name: 'Batman'
};
const hero2 = {
  name: 'Batman'
};
const hero3 = {
  name: 'Joker'
};
isHeroEqual(hero1, hero2); // => true
isHeroEqual(hero1, hero3); // => false
```

### 3. Shallow equality
During shallow equality check of objects you get the list of properties (using Object.keys()) of both objects, then check the properties' values for equality.  

```js
function shallowEqual(object1, object2) {
  const keys1 = Object.keys(object1);
  const keys2 = Object.keys(object2);
  if (keys1.length !== keys2.length) {
    return false;
  }
  for (let key of keys1) {
    if (object1[key] !== object2[key]) {
      return false;
    }
  }
  return true;
}
```

```js
const hero1 = {
  name: 'Batman',
  realName: 'Bruce Wayne'
};
const hero2 = {
  name: 'Batman',
  realName: 'Bruce Wayne'
};
const hero3 = {
  name: 'Joker'
};
shallowEqual(hero1, hero2); // => true
shallowEqual(hero1, hero3); // => false
```

shallowEqual(hero1, hero2) returns true because the objects hero1 and hero2 have the same properties (name and realName) with the same values.

On the other side, shallowEqual(hero1, hero3) returns false since hero1 and hero3 have different properties.  

```js
const hero1 = {
  name: 'Batman',
  address: {
    city: 'Gotham'
  }
};
const hero2 = {
  name: 'Batman',
  address: {
    city: 'Gotham'
  }
};
shallowEqual(hero1, hero2); // => false
```

But objects in JavaScript can be nested. In such a case, unfortunately, the shallow equality doesn't work well.

Shallow Equalはネストしたobjectだと正しく機能しない  

The nested objects hero1.address and hero2.address are different object instances. Thus the shallow equality considers that hero1.address and hero2.address are different values.  

### 4. Deep equality
The deep equality is similar to the shallow equality, but with one difference. During the shallow check, if the compared properties are objects, a recursive shallow equality check is performed on these nested objects.  

```js
function deepEqual(object1, object2) {
  const keys1 = Object.keys(object1);
  const keys2 = Object.keys(object2);
  if (keys1.length !== keys2.length) {
    return false;
  }
  for (const key of keys1) {
    const val1 = object1[key];
    const val2 = object2[key];
    const areObjects = isObject(val1) && isObject(val2);
    if (
      areObjects && !deepEqual(val1, val2) ||
      !areObjects && val1 !== val2
    ) {
      return false;
    }
  }
  return true;
}
function isObject(object) {
  return object != null && typeof object === 'object';
}
```

```js
const hero1 = {
  name: 'Batman',
  address: {
    city: 'Gotham'
  }
};
const hero2 = {
  name: 'Batman',
  address: {
    city: 'Gotham'
  }
};
deepEqual(hero1, hero2); // => true
```
The deep equality function correctly determines that hero1 and hero2 have the same properties and values, including the equality of the nested objects hero1.address and hero2.address.  

## React.memoの仕組み
[React.memo を使ったパフォーマンス最適化について](https://numb86-tech.hatenablog.com/entry/2019/12/20/222412)  

メモ化とは、何らかの計算によって得られた値を記録しておき、その値が再度必要になったときに、再計算することなく値を得られるようにすることである。
メモ化によって都度計算する必要がなくなるため、パフォーマンスの向上が期待できる。
React.memoとは言ってみれば、メモ化を利用して不要な処理をスキップするための機能であり、それを活用することが、React アプリのパフォーマンス最適化の基本的な戦略である。  

JavaScript においてはメモ化にはもうひとつ重要な役割がある。それは、同じ参照のオブジェクトを得られるようになるということ。
記録しておいたオブジェクトを取り出すので、「値が一緒だが参照が異なる別のオブジェクト」になってしまうことを防げる。
JavaScript のオブジェクトの等価性は「参照が同じかどうか」で判断することが多く、React も例外ではないため、これは重要な意味を持つ。  

React.memoでは、コンポーネントが返した React 要素を記録する。
そして、再レンダーが行われそうになったときは本当に再レンダーが必要なのかチェックし、必要なときだけ再レンダーし、必要ないときは記録しておいた React 要素を再利用することで、再レンダーを最低限に抑える。

再レンダーが必要かどうかは、propsの等価性によって判断される。
新しく渡されたpropsを前回のpropsと比較し、同じであると見なせば再レンダーは不要であると判断し、異なると見なせば再レンダーを行う。  

デフォルトでは、等価性の判断にshallow compareを使う。これは、オブジェクトの1階層のみを比較することである。
先程の例のChildではpropsは常に{}であるため、shallow compareによって等価であると判断される。  

## Improving React application performance: React.memo vs useMemo
[Improving React application performance: React.memo vs useMemo](https://blog.openreplay.com/improving-react-application-performance-react-memo-vs-usememo)  

### React.memo
Ideally, this should fix the issue but in this case, it does not. React.memo uses a shallow comparison of the component props and because of how JavaScript works, comparing objects shallowly will always return false even if they have the same values.  

This is why React.memo also takes in a second argument. This argument is a custom equality check function that you, the developer, uses to decide if the component should render   

```js
const barGraphPropsAreEqual = (prevProps, newProps) => {
    ...some custom logic to determine if the props are equal
}
// we update our React.memo usage
export const memoisedBarGraph = React.memo(BarGraph, barGraphPropsAreEqual)
```

When to use React.memo You can use React.memo if your component —

- Will always render the same thing given the same props (i.e, if you have to make a network call to fetch some data and there’s a chance the data might not be the same, do not use it)
- Renders expensively (i.e, it takes at least 100ms to render)
- Renders often

### useMemo

This is where useMemo shines. If we have a variable inside a component that is expensive to compute, we can use useMemo to reduce the number of times this has to happen by memoizing the computed variable.  

```js
const Encrypter = ({ dataToEncrypt, encryptKey }) => {
    const encryptedData = useMemo(
    () => encrypt(dataToEncrypt, secretKey),
    [dataToEncrypt, encryptKey]
    );
    ...render method
}
```

When to use useMemo Before deciding to use useMemo, make sure that —

- You have profiled the component and determined if it’s calculating an expensive value in every render
- There are no side effects (e.g async calls) in your useMemo hook because it runs during rendering and all side effects belongs in the useEffect hook.
- You do not break the rules of React Hooks

### Which one should I pick?
Now that we have a solid understanding of both React.memo and useMemo, deciding which one to use should be simple.

If you’re memoizing a whole component, use React.memo
If you’re memoizing a value inside a functional component, use useMemo




