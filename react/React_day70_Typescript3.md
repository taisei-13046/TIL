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



































