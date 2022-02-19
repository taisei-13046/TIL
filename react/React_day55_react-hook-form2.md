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

### useRefとcreateRefの違い
参照  
[What's the difference between `useRef` and `createRef`?](https://stackoverflow.com/questions/54620698/whats-the-difference-between-useref-and-createref)  
[demo](https://codesandbox.io/s/1rvwnj71x3?file=/src/index.js)  

- createRefは常に新しいrefを作成する  
- useRefは毎回のレンダリングで同じrefを返す  

> The difference is that createRef will always create a new ref. In a class-based component, you would typically put the ref in an instance property during construction (e.g. this.input = createRef()). You don't have this option in a function component. useRef takes care of returning the same ref each time as on the initial rendering.

↓ 翻訳

> 違いは、createRef は常に新しい ref を作成することです。 クラスベースのコンポーネントでは、通常、構築中にインスタンスプロパティに ref を配置します（例：this.input = createRef（））。 関数コンポーネントにはこのオプションはありません。 useRef は、最初のレンダリング時と同じ ref を毎回返します。

## React Hook Form
### useController 
Controller コンポーネントの機能を扱いやすいようにフック化したもの  
コンポーネント内部でフックを利用できるので、フィールドタグを内包したコンポーネントを再利用しやすくなります。
























