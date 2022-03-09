## やったこと
サバイバルTypescriptを読む！

### シンボル型 (symbol type)
JavaScriptのシンボル型(symbol type)は、プリミティブ型の一種で、その値が一意になる値です。論理型や数値型は値が同じであれば、等価比較がtrueになります。一方、シンボルはシンボル名が同じであっても、初期化した場所が違うとfalseになります。

```ts
const s1 = Symbol("foo");
const s2 = Symbol("foo");
console.log(s1 === s1);
true
console.log(s1 === s2);
false
```














