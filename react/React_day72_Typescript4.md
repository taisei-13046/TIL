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














