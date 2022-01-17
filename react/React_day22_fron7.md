## やったこと
フロントの最終課題とハッカソンの準備  

### display: inline-flexって何？？
Molecules/NewsAndEventsMoreにおいて、`a`タグの範囲がdisplay:flexにすると横全体になってしまう現象が起きた。  
それを解決するのがdisplay: inline-flexであったが、なぜそれで解決されるのかわからなかった。  
`display: inline-flex`とは、要素をインラインレベルのflexコンテナとして定義する  
とあるがそのまますぎる。  

`display: inline-flex`は`display: inline-box`の拡張版(flexの要素も持つ)  
とあったが、改めてinline-boxの理解が甘い気がした  

<img width="733" alt="スクリーンショット 2022-01-16 23 29 51" src="https://user-images.githubusercontent.com/78260526/149664147-df72c7dd-9a9e-4668-ae27-81546af1fb37.png">

inline-block要素の特徴
- 横幅と高さが指定できる
- ブロック要素と同じ余白のつき方をする
- 横幅の初期値が内容で決まる
- 前後の要素と横に並ぶ

このうちの**横幅の初期値が内容で決まる**が今回のポイントだった。  

## どういう流れで開発していく？

まずは、必要になる色やfont-size, line-heightをそれぞれ定義する  
`client/src/style_vars`にcssの定数を定義していく  
`client/src/style_vars/`にcolor, font-family, font-size, line-heightを定義する  

仮に、font-sizeの場合は、  
```ts
const FONTSIZE = {
  H1: 48,
  H2: 40,
  H3: 32,
  XL: 25,
  L: 18,
  M: 16,
  S: 12,
  XS: 10,
  XXS: 8,
} as const;
type FONTSIZE = typeof FONTSIZE[keyof typeof FONTSIZE];
export default FONTSIZE;
```
このようにexportする  

ここで気になったのが
- `type FONTSIZE = typeof FONTSIZE[keyof typeof FONTSIZE];`が何をしているのか
- `as const`とは
- export default でtypeも値もexportしてる

### `type FONTSIZE = typeof FONTSIZE[keyof typeof FONTSIZE];`が何をしているのか
[[Typescript] typeof "object value" [keyof typeof "object value"] の動作を丁寧に解説してみる](https://qiita.com/saba_can00/items/bdefb28a1873658cf5d9)  
この１行は、
- オブジェクトの型を取得する typeof operator
- 型のプロパティ名のUnionTypeを表現する keyof operator
- ある型のプロパティの型を表現できる`T[K]`
に分類することができる  

### 1.オブジェクトの型を取得する typeof operator
typeof operatorを利用すると下記のように特定の値の型を取得することができる  
```ts
let test1 = 'test1'
//↓は'string'に'number'を代入しているので、コンパイルエラーになる
let test2: typeof test1 = 2 
```

### 2. 型のプロパティ名のUnionTypeを表現する keyof operator
ある型Tのプロパティ名のUnionTypeを取得する演算子  
```ts
interface Person {
  name: string;
  age: number;
  location: string;
}

type K1 = keyof Person; // "name" | "age" | "location"
type K2 = keyof Person[]; // "length" | "push" | "pop" | "concat" | ...
```

### 3. ある型のプロパティの型を表現できるT[K]
`T[K]`はindexed access types、またはlookup typesと呼ばれ、オブジェクトのプロパティにアクセスするような記法でプロパティの型を取得できる  

```ts
interface Person {
  name: string;
  age: number;
  location: string;
}

type P1 = Person["name"] // string
type P2 = Person["name" | "age"] // string | number
type P3 = Person[keyof Person]// string | number
```

### `as const`とは？？
[constアサーション「as const」 (const assertion)](https://typescriptbook.jp/reference/values-types-variables/const-assertion)  
オブジェクトリテラルの末尾にas constを記述すればプロパティが**readonly**でリテラルタイプで指定した物と同等の扱いになる  

#### readonlyとconst assertionの違い
- readonlyはプロパティごとにつけられる
  - `as const`は全てのプロパティが対象になる
  - const assertionはすべてのプロパティを固定する



