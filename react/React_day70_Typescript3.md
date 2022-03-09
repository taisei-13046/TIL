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





























