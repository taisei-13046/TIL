## やったこと
サバイバルTypescriptを読む3

### ユニオン型 (union type)
TypeScriptのユニオン型(union type)は「いずれかの型」を表現するものです。

|は型のリストの冒頭に置くこともできます。型ごとに改行するときに、列が揃うので便利です。

```ts
type ErrorCode =
  | 400
  | 401
  | 402
  | 403
  | 404
  | 405;
```

#### ユニオン型と絞り込み
ユニオン型string | nullがstringなのかnullなのかを判定したいときは、TypeScriptの絞り込み(narrowing)を用います。絞り込みをするにはいくつかの方法がありますが、代表例はif文です。条件分岐で変数が文字列型かどうかをチェックすると、同じ変数でも分岐内ではstring | null型がstring型だと判定されます。

```ts
const maybeUserId: string | null = localStorage.getItem("userId");
const userId: string = maybeUserId; // nullかもしれないので、代入できない。
if (typeof maybeUserId === "string") {
  const userId: string = maybeUserId; // この分岐内では文字列型に絞り込まれるため、代入できる。
}
```

### 判別可能なユニオン型 (discriminated union)

TypeScriptの判別可能なユニオン型は、ユニオンに属する各オブジェクトの型を区別するための「しるし」がついた特別なユニオン型です。オブジェクト型からなるユニオン型を絞り込む際に、分岐ロジックが複雑になる場合は、判別可能なユニオン型を使うとコードの可読性と保守性がよくなります。  

TypeScriptの判別可能なユニオン型(discriminated union)はユニオン型の応用です。判別可能なユニオン型は、タグ付きユニオン(tagged union)や直和型と呼ぶこともあります。

判別可能なユニオン型は次の特徴を持ったユニオン型です。

1. オブジェクト型で構成されたユニオン型
2. 各オブジェクト型を判別するためのプロパティ(しるし)を持つ  
このプロパティのことをディスクリミネータ(discriminator)と呼ぶ  
3. ディスクリミネータの型はリテラル型などであること
4. ディスクリミネータさえ有れば、各オブジェクト型は固有のプロパティを持ってもよい

```ts
type UploadStatus = InProgress | Success | Failure;
type InProgress = { type: "InProgress"; progress: number };
type Success = { type: "Success" };
type Failure = { type: "Failure"; error: Error };
```

#### ディスクリミネータに使える型
ディスクリミネータに使える型は、リテラル型とnull、undefinedです。

- リテラル型
  - 文字列リテラル型: (例)"success"、"OK"など
  - 数値リテラル型: (例)1、200など
  - 論理値リテラル型: trueまたはfalse
- null
- undefined

### インターセクション型 (intersection type)
考え方はユニオン型と相対するものです。ユニオン型がどれかを意味するならインターセクション型はどれもです。言い換えるとオブジェクトの定義を合成させることを指します。

インターセクション型を作るためには合成したいオブジェクト同士を&で列挙します。  

```ts
type TwoDimensionalPoint = {
  x: number;
  y: number;
};
type Z = {
  z: number;
};
type ThreeDimensionalPoint = TwoDimensionalPoint & Z;
const p: ThreeDimensionalPoint = {
  x: 0,
  y: 1,
  z: 2,
};
```

#### インターセクション型を使いこなす
必須とそうでないパラメータのタイプエイリアスに分離する

```ts
type Mandatory = {
  id: string;
  active: boolean;
  balance: number;
  surname: string;
  givenName: string;
  email: string;
};
type Optional = {
  index: number;
  photo: string;
  age: number;
  company: string;
  phoneNumber: string;
  address: string;
};
```

`Required<T>, Partial<T>`をつける
Mandatoryは`Required<T>`を、Optionalは`Partial<T>`をつけます。

```ts
type Mandatory = Required<{
  id: string;
  active: boolean;
  balance: number;
  surname: string;
  givenName: string;
  email: string;
}>;
type Optional = Partial<{
  index: number;
  photo: string;
  age: number;
  company: string;
  phoneNumber: string;
  address: string;
}>;
```

```ts
type Parameter = Mandatory & Optional;
```

### 型エイリアス (type alias)
TypeScriptでは、型に名前をつけられます。名前のついた型を型エイリアス(タイプエイリアス; type alias)と呼びます。

#### 型エイリアスの使い道
型エイリアスは同じ型を再利用したいときに使うと便利です。型の定義が一箇所になるため、保守性が向上します。

また、型に名前を与えることで可読性が上がる場合があります。型に名前があると、その型が何を意味しているのかがコードの読み手に伝わりやすくなります。

#### interfaceとの違い
![スクリーンショット 2022-03-10 10 24 39](https://user-images.githubusercontent.com/78260526/157568691-77ecd9b1-0a11-437b-b54e-9d6b5f389834.png)  

### 型アサーション「as」(type assertion)
TypeScriptには、型推論を上書きする機能があります。その機能を型アサーション(type assertion)と言います。

TypeScriptコンパイラーはコードをヒントに型を推論してくれます。その型推論は非常に知的ですが、場合によってはコンパイラーよりもプログラマーがより正確な型を知っている場合があります。そのような場合は、型アサーションを用いるとコンパイラーに型を伝えることができます。型アサーションはコンパイラに「私を信じて！私のほうが型に詳しいから」と伝えるようなものです。  

型アサーションの書き方は2つあります。1つはas構文です。

```ts
const value: string | number = "this is a string";
const strLength: number = (value as string).length;
```

もう1つはアングルブランケット構文(angle-bracket syntax)です。

```ts
const value: string | number = "this is a string";
const strLength: number = (<string>value).length;
```

どちらを用いるかは好みですが、アングルブランケット構文はJSXと見分けがつかないことがあるため、as構文が用いられることのほうが多いです。

型アサーションとキャストの違い
型アサーションは、他の言語のキャストに似ています。キャストとは、実行時にある値の型を別の型に変換することです。型アサーションは、実行時に影響しません。値の型変換はしないのです。あくまでコンパイル時にコンパイラーに型を伝えるだけです。コンパイラーはその情報を手がかりに、コードをチェックします。型アサーションはキャストではないため、TypeScriptでは型アサーションをキャストとは呼ばないことになっています。実行時に型変換をするには、そのためのロジックを書く必要があります。

大いなる力には大いなる責任が伴う
型アサーションには、コンパイラーの型推論を上書きする強力さがあります。そのため、プログラマーは型アサーションによってバグを産まないように十分注意する必要があります。型に関することはできるだけ、コンパイラーの型推論に頼ったほうが安全なので、型アサーションは、やむを得ない場合にのみ使うべきです。

型アサーションを使う必要が出てきたら、それよりも先に、型ガードやユーザー定義型ガードで解決できないか検討してみるとよいでしょう。

### constアサーション「as const」 (const assertion)
オブジェクトリテラルの末尾にas constを記述すればプロパティがreadonlyでリテラルタイプで指定した物と同等の扱いになります。

```ts
const pikachu = {
  name: "pikachu",
  no: 25,
  genre: "mouse pokémon",
  height: 0.4,
  weight: 6.0,
} as const;
```

#### readonlyとconst assertionの違い
- readonlyはプロパティごとにつけられる
  - const assertionはオブジェクト全体に対する宣言なので、すべてのプロパティが対象になりますが、readonlyは必要なプロパティのみにつけることができます。
- const assertionは再帰的にreadonlyにできる
- const assertionはすべてのプロパティを固定する

#### typeof型演算子

```ts
const point = { x: 135, y: 35 };
type Point = typeof point;
```

このPoint型は次のような型になります。

```ts
type Point = {
  x: number;
  y: number;
};
```





















