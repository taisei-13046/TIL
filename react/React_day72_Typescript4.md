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

### anyとunknownの違い
any, unknown型はどのような値も代入できます。

any型に代入したオブジェクトのプロパティ、メソッドは使用することができます。

```ts
console.log(any4.toFixed());
// @log: 1
console.log(any5.length);
// @log: 18
console.log(any6.name);
// @log: "origin"
```

一方、unknown型に代入したオブジェクトのプロパティ、メソッドは使用することができません。使用できないどころか、実行することができません。
```ts
console.log(unknown4.toFixed());
Object is of type 'unknown'.
console.log(unknown5.length);
Object is of type 'unknown'.
console.log(unknown6.name);
Object is of type 'unknown'.
```

## 関数
### 関数宣言と巻き上げ (hoisting)
JavaScriptの関数宣言と関数式の違いが現れるひとつの例は巻き上げ(hoisting)です。関数宣言には巻き上げがあり、関数式には巻き上げがありません。  

まずは関数宣言の例を見てみましょう。次のコードは、3行目にhello関数の関数宣言があります。そして、その宣言の前でhello関数を実行しています。

```ts
hello();
function hello() {
  console.log("Hello World");
}
```

このコードは、hello関数の定義行より前でその関数を呼び出しているのに、エラーにはならず問題なく"Hello World"が出力されます。これは関数宣言には巻き上げがあるためです。

次に関数式の例を見てみましょう。下のコードはhello関数を関数式を使って定義するようにしたものです。

```ts
hello();
const hello = function () {
  console.log("Hello World");
};
```

このコードをJavaScriptとして実行してみると、1行目で「ReferenceError: Cannot access 'hello' before initialization」というエラーが起こります。関数式で関数を定義した場合は巻き上げがないため、このようなエラーが発生します。


### 関数式とアロー関数の違い
[JavaScript: 通常の関数とアロー関数の違いは「書き方だけ」ではない。異なる性質が10個ほどある。](https://qiita.com/suin/items/a44825d253d023e31e4d)  

#### 違い1: thisの指すもの
通常関数にはthisがあり、thisが何を指すのかは、通常関数を実行したタイミングで決まります。

アロー関数には、それ自体が保有するthisはなく、関数の外のthisを関数内で参照できるだけです。レキシカルスコープのthisを参照します。つまり、アロー関数は定義したときに、thisが指すものがひとつに決まり、どうやって関数が実行されるかに左右されなくなります。

#### 違い2: newできるかどうか
通常関数はnewすることができますが、アロー関数はnewすることができません。つまり、アロー関数はコンストラクタになることができません。  

したがって、通常関数はclassでextendsできますが、アロー関数はできません:

```ts
function regular() {}
const arrow = () => {}

class Foo extends regular {}
class Bar extends arrow {} //=> TypeError: Class extends value () => {} is not a constructor or null
```

#### 違い3: call, apply, bindの振る舞い
通常関数は、call, apply, bindメソッドの第一引数で、その関数のthisを指すオブジェクトを指定することができます。アロー関数は、指定しても無視されます。

#### 違い4: prototypeプロパティの有無
通常関数にはprototypeプロパティがありますが、アロー関数にはありません。

#### 違い5: argumentsの有無
通常関数は、argumentsで引数リストを参照できますが、アロー関数ではargumentsが定義されていないため、引数リストを参照できません。アロー関数のargumentsはレキシカルスコープ変数のargumentsを参照するだけです。  

ちなみに、アロー関数で引数リストを参照する場合は、可変長引数を定義する方法があります:

```ts
const arrow = (...arguments) => {
    console.log(arguments)
}
arrow(1, 2, 3) //=> [ 1, 2, 3 ]
```

#### 違い6: 引数名の重複
通常関数は、引数名の重複が許されますが、アロー関数は同じ引数名があるとエラーになります:

#### 違い7: 巻き上げ(hoisting)
通常関数は、var相当の巻き上げ(hoisting)が起こります。なので、関数定義前に呼び出しのコードを書いても、関数が実行できます:

```ts
regular() // エラーにならない
function regular() {}
```

アロー関数は巻き上げが起こりません:

```ts
arrow() // ReferenceError: Cannot access 'arrow' before initialization
const arrow = () => {}
```

#### 違い8: ジェネレータ関数
通常関数はジェネレータ関数を定義できますが、アロー関数はジェネレータ関数を定義する構文がそもそもありません。  

#### 違い9: 同じ関数名での再定義
通常関数は、同じ名前の関数を定義できます。最後の関数で上書きされます。

アロー関数はletやconstの仕様上、同じ関数名で定義を上書きすることができません:

#### 違い10: super, new.targetの有無
アロー関数には束縛されたsuperやnew.targetが無い

### 関数は値
JavaScriptの関数は値です。つまり、PHPのような他の言語と比べると特別扱いの度合いが少ないです。  

#### 関数のスコープ
関数は値なので、関数名のスコープも変数と同じようにスコープの概念があります。たとえば、関数スコープの中で定義された関数は、そのローカルスコープでのみ使うことができます。

### 関数はオブジェクト
JavaScriptの関数はオブジェクトです。したがって、関数にプロパティを持たせることができます。

```ts
function hello() {
  return "Hello World";
}
hello.prop = 123;
```

### 戻り値がない関数とvoid型 (void type)
#### undefined型とvoid型の違い
JavaScriptの関数では、戻り値がない場合は値としてundefinedを返します。またTypeScriptにはundefined型もあります。TypeScriptの型上の意味としては、undefined型とvoid型は同じです。したがって、戻り値の型注釈にundefinedを用いることもできます。ただし、戻り値型がundefined型の場合は、return undefinedが必要です。  [

void型は関数戻り値の型注釈にだけ使うのが普通なので、変数の型注釈に使うことはまずありませんが、もしも変数の型注釈にvoid型を使った場合は、異なる挙動になります。undefined型の変数をvoid型の変数に代入することができる一方、void型の変数をundefined型の変数に代入することはできません。  

### オプション引数
オプション引数は必ず最後に書かなければいけません。つまり、次のようにオプション引数より後ろに普通の引数を書くことはできません。

```ts
function distance(p1?: Point, p2: Point): number {
  // ...
}
// A required parameter cannot follow an optional parameter.
```

### キーワード引数とOptions Objectパターン
JavaScriptやTypeScriptの関数には、Pythonにあるキーワード引数のような機能はありません。その代わり、分割代入引数を応用することで、キーワード引数と同じようなことができます。

キーワード引数(keyword argument)は、Pythonの機能です。関数呼び出し時に、値だけを指定するのではなく、引数名を使って「名前=値」の形式で引数を指定する方法です。

キーワード引数の仕様上の特徴は、位置引数(positional argument)と異なり、関数呼び出し側が引数の順番を自由に変えられるところです。

```python
# Pythonコード
func(z=3, y=2, x=1)  # => 1 2 3
```

JavaScriptやTypeScriptにはキーワード引数のような言語仕様はありませんが、Options Objectパターンというデザインパターンで似たようなことができます。Options Objectパターンは複数の位置引数を受け取る代わりに、ひとつのオブジェクトを引数に受け取るように設計された関数を言います。

Options Objectパターンの利点
Options Objectパターンの利点は次の3つがあります。

1. 引数の値が何を指すのか分かりやすい
2. 引数追加時に古いコードを壊さない
3. デフォルト引数が省略できる








