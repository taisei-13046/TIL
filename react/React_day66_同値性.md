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













