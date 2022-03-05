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




















