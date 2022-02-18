## やったこと
React Hook Formの続き

## interfaceのOmitについて
[“omit an interface typescript” Code Answer](https://www.codegrepper.com/code-examples/typescript/omit+an+interface+typescript)  
[Omitで複数のプロパティを同時に除外する](https://qiita.com/xx2xyyy/items/226319f4239016676bea)  

```ts
interface A {
    x: string
}

interface B extends Omit<A, 'x'> {
  x: number
}
```

`Omit<A, 'x'>`でAからxを除外することができる  

#### 複数プロパティを同時に除外する場合
```ts
Omit<HOGE,"fuga" | "piyo">
```
このように除外できる  





