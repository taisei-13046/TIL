## やったこと
サバイバルTypescriptを読む  

### neverを使った網羅性チェック
neverの何も代入できないという特性は、網羅性チェック(exhaustiveness check)に応用できます。網羅性チェックとは、ユニオン型を分岐処理するとき、ロジックがすべてのパターンを網羅しているかをコンパイラにチェックさせることを言います。

たとえば、3パターンのユニオン型があるとします。

```ts
type Extension = "js" | "ts" | "json";
```

#### 網羅性チェックの基本
網羅性チェックを行うには、default分岐で網羅性をチェックしたい値をnever型に代入します。すると、TypeScriptが代入エラーの警告を出すようになります。  

```ts
function printLang(ext: Extension): void {
  switch (ext) {
    case "js":
      console.log("JavaScript");
      break;
    case "ts":
      console.log("TypeScript");
      break;
    default:
      const exhaustivenessCheck: never = ext;
      break;
  }
}
```

#### 例外による網羅性チェック
一歩進んで網羅性チェック用の例外クラスを定義するのがお勧めです。このクラスは、コンストラクタ引数にnever型を取る設計にします。

網羅性チェック関数
```ts
class ExhaustiveError extends Error {
  constructor(value: never, message = `Unsupported type: ${value}`) {
    super(message);
  }
}
```

この例外をdefault分岐で投げるようにします。コンストラクタに網羅性をチェックしたい引数を渡します。こうしておくと、網羅性が満たされていない場合、TypeScriptが代入エラーを警告します。

```ts
function printLang(ext: Extension): void {
  switch (ext) {
    case "js":
      console.log("JavaScript");
      break;
    case "ts":
      console.log("TypeScript");
      break;
    default:
      throw new ExhaustiveError(ext);
Argument of type 'string' is not assignable to parameter of type 'never'.
  }
}
```

例外にしておく利点は2つあります。

1. noUnusedLocalsに対応可能
2. 実行時を意識したコードになる

### 制御フロー分析と型ガードによる型の絞り込み

#### 制御フロー分析
TypeScriptはifやforループなどの制御フローを分析することで、コードが実行されるタイミングでの型の可能性を判断しています。  

```ts
function showMonth(month: string | number) {
  if (typeof month === "string") {
    console.log(month.padStart(2, "0"));
  }
}
```

#### 型ガード
制御フローの説明において、型の曖昧さを回避するためにif(typeof month === "string")という条件判定で変数の型を判定して型の絞り込みを行いました。

このような型チェックのコードを型ガードと呼びます。

次の例ではtypeofでmonth変数の型をstring型と判定しています。

```ts
function showMonth(month: string | number) {
  if (typeof month === "string") {
    console.log(month.padStart(2, "0"));
  }
}
```


#### instanceof
typeofでインスタンスを判定した場合はオブジェクトであることまでしか判定ができません。
特定のクラスのインスタンスであることを判定する型ガードを書きたい場合はinstanceofを利用します。

```ts
function getMonth(date: string | Date) {
  if (date instanceof Date) {
    console.log(date.getMonth() + 1);
  }
}
```

#### in
特定のクラスのインスタンスであることを明示せず、in演算子でオブジェクトが特定のプロパティを持つかを判定する型ガードを書くことで型を絞り込むこともできます。

```ts
interface Wizard {
  castMagic(): void;
}
interface SwordMan {
  slashSword(): void;
}
 
function attack(player: Wizard | SwordMan) {
  if ("castMagic" in player) {
    player.castMagic();
  } else {
    player.slashSword();
  }
}
```

#### ユーザー定義の型ガード関数
型ガードはインラインで記述する以外にも関数として定義することもできます。

```ts
function isWizard(player: Player): player is Wizard {
  return "castMagic" in player;
}
function attack(player: Wizard | SwordMan) {
  if (isWizard(player)) {
    player.castMagic();
  } else {
    player.slashSword();
  }
}
```

#### 型ガードの変数代入
型ガードに変数を使うこともできます。
ただし、この文法は TypeScript4.4 以降のみで有効なため、使用する場合はバージョンに注意してください。

```ts
function getMonth(date: string | Date) {
  const isDate = date instanceof Date;
  if (isDate) {
    console.log(date.getMonth() + 1);
  }
}
```



















