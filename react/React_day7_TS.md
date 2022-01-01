## やったこと
ReactとTSについてまとめた  


### 関数コンポーネント
[React + TypeScript: ReactでTypeScriptを使うとき基本として知っておきたいこと](https://qiita.com/FumioNonaka/items/4361d1cdf34ffb5a5338)  


FCとVFCの違いは**childrenを暗黙的に受け付けるかどうか**  
もしもVFCを使用してChildrenをpropsで型指定する場合には、
```js
children: React.ReactNode
```  
このように指定する必要がある
しかしながら、FCとVFCはあまり使う必要はないのではないかという記事もある  
[React.FC と React.VFC はべつに使わなくていい説 ](https://kray.jp/blog/dont-have-to-use-react-fc-and-react-vfc/)  

### .ts と .tsxの違い
`.ts`
- 純粋なTypeScriptファイル
- **JSX要素の追加をサポートしない**
.tsファイルでは、JSXの記法を使用できない!!

`.tsx`
- JSXを含むファイル
- 型アサーションの記法として value as type と `<type>value` の2通りあるが、後者は .tsx には書けない  
（ `<>` はJSXタグのマーカーであるため）  

そしたら、全て.tsxにすればいいのではないかと思ってしまうが、それは良くない。  
#### .tsファイルでは明示的にJSXを使わないことを示すために使用するべき!!  

参考資料 [.ts拡張子と.tsx拡張子](https://qiita.com/y-hira18/items/f67cfc45a70c7c25708a)

### Typescriptのdocument

#### Number型
数値計算が有効な数値で表現できない場合、JavaScriptは特別な`NaN`値を返す  
```js
console.log(Math.sqrt(-1)); // NaN


// これはしないでください
console.log(NaN === NaN); // false!!

// こうしてください
console.log(Number.isNaN(NaN)); // true
```
等価演算子はNaN値では機能しない。  
代わりに`Number.isNaN`を使用  

#### Truthy
JavaScriptは、特定の場所(例えば、if 条件文とbooleanの&& || オペレータ)で、  
Trueと評価される値(truthy)の概念を持っている  

![スクリーンショット 2022-01-01 19 52 08](https://user-images.githubusercontent.com/78260526/147848972-ac41c622-b258-44ea-a340-9c89a12f0868.png)  

一般的に、booleanとして扱われる値を、それを本当のboolean(true|false)に明示的に変換することは、良いこと。  
`!!`を使って値を本当のbooleanに簡単に変換できる  

```js
// 値を他のものに移す
const hasName = !!name;

// オブジェクトのメンバとして利用する
const someObj = {
  hasName: !!name
}

// 例： ReactJS JSX
{!!someName && <div>{someName}</div>}
```

#### スプレッド構文(Spread Syntax)
...fooの形で記述され、配列やオブジェクトの要素を文字通り展開する構文  
```js
const point2D = {x: 1, y: 2};
/** point2Dのプロパティと、`z`のプロパティを持った新しいオブジェクトを作成します */
const point3D = {...point2D, z: 3};
```
＊JavaScript エンジンには、引数の個数に上限がある。関数呼び出しでのスプレッド構文では、引数の個数がその上限を超えてしまう可能性に留意しなくてはいけない。  
[MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Spread_syntax)  

#### TSとPromise
TypeScriptが素晴らしい点は、それがPromiseチェーンを通じて流れる値を理解してくれること  
```js
Promise.resolve(123)
    .then((res) => {
         // res は `number` 型と推論される
         return true;
    })
    .then((res) => {
        // res は `boolean` 型と推論される

    });
```
Promiseを返す可能性のある関数呼び出しも理解してくれる
```js
function iReturnPromiseAfter1Second(): Promise<string> {
    return new Promise((resolve) => {
        setTimeout(() => resolve("Hello world!"), 1000);
    });
}

Promise.resolve(123)
    .then((res) => {
        // res は `number` 型と推論される
        return iReturnPromiseAfter1Second(); // `Promise<string>`を返す
    })
    .then((res) => {
        // res は `string` 型と推論される
        console.log(res); // Hello world!
    });
```

**並列制御フロー(Parallel control flow)**  
複数の非同期処理を実行し、すべてのタスクが終わったタイミングで何らかの処理を行いたいケースがある  
Promiseは静的な`Promise.all関数`を提供する  
この関数は、n個のPromiseがすべて完了するまで待つことができる  
n個のPromiseの配列を渡すと、n個の解決された値の配列を返す  
```js
// 何らかのデータをサーバから読み込むことを再現する処理
function loadItem(id: number): Promise<{ id: number }> {
    return new Promise((resolve) => {
        console.log('loading item', id);
        setTimeout(() => { // サーバーからのレスポンス遅延を再現
            resolve({ id: id });
        }, 1000);
    });
}

// Promiseチェーン
let item1, item2;
loadItem(1)
    .then((res) => {
        item1 = res;
        return loadItem(2);
    })
    .then((res) => {
        item2 = res;
        console.log('done');
    }); // 全体で 2秒 かかる

// 並列処理
Promise.all([loadItem(1), loadItem(2)])
    .then((res) => {
        [item1, item2] = res;
        console.log('done');
    }); // 全体で 1秒 かかる
```
複数の非同期タスクを実行するが、これらのタスクの内1つだけが完了すれば良いケースもある  
Promiseは、このユースケースに対して`Promise.race`というStatic関数を提供している  
```js
var task1 = new Promise(function(resolve, reject) {
    setTimeout(resolve, 1000, 'one');
});
var task2 = new Promise(function(resolve, reject) {
    setTimeout(resolve, 2000, 'two');
});

Promise.race([task1, task2]).then(function(value) {
  console.log(value); // "one"
  // 両方ともresolveされるが、task1の方が早く終わる
});
```

#### 宣言空間
- 型宣言空間
- 変数宣言空間

型宣言空間には型アノテーションとして使用できるものが含まれている  
(例)
```ts
class Foo {};
interface Bar {};
type Bas = {};
```
`interface Bar`を持っていても、変数宣言空間に宣言されないので変数として使うことはできない  
```ts
interface Bar {};
var bar = Bar; // ERROR: "'Bar'が見つかりません"
```

変数宣言空間には、変数として使用できるものがある  
```ts
var foo = 123;
var bar: foo; // ERROR: "'foo'が見つかりません"
```

#### 型ガード
Type Guardを使用すると、条件ブロック内のオブジェクトの型を制限することができる  
`typeof`  
条件付きブロックでこれらを使用すると、TypeScriptはその条件ブロック内で異なる変数の型を理解する  
```ts
function doSomething(x: number | string) {
    if (typeof x === 'string') { // Within the block TypeScript knows that `x` must be a string
        console.log(x.subtr(1)); // Error, 'subtr' does not exist on `string`
        console.log(x.substr(1)); // OK
    }
    x.substr(1); // Error: There is no guarantee that `x` is a `string`
}
```
`instanceof`  
instanceof 演算子は、オブジェクトが自身のプロトタイプにコンストラクタの prototype プロパティを持っているかを確認する  
戻り値はbool値  

```ts
function doStuff(arg: Foo | Bar) {
    if (arg instanceof Foo) {
        console.log(arg.foo); // OK
        console.log(arg.bar); // Error!
    }
    if (arg instanceof Bar) {
        console.log(arg.foo); // Error!
        console.log(arg.bar); // OK
    }

    console.log(arg.common); // OK
    console.log(arg.foo); // Error!
    console.log(arg.bar); // Error!
}
```

`in`
```ts
interface A {
  x: number;
}
interface B {
  y: string;
}

function doStuff(q: A | B) {
  if ('x' in q) {
    // q: A
  }
  else {
    // q: B
  }
}
```

strictNullChecksを使用したnullとundefinedのチェック  
TypeScriptは十分賢いので、次のように`== null` / `!= null`チェックをすることでnullとundefinedの両方を排除できる  
```ts
function foo(a?: number | null) {
  if (a == null) return;

  // a is number now.
}
```

#### Readonly
TypeScriptの型システムでは、インターフェース上の個々のプロパティをreadonlyとしてマークすることができる  
```ts
function foo(config: {
    readonly bar: number,
    readonly bas: number
}) {
    // ..
}

let config = { bar: 123, bas: 123 };
foo(config);
// You can be sure that `config` isn't changed 🌹
```

Readonly型はT型をとり、そのすべてのプロパティをreadonlyとマークする  
```ts
type Foo = {
  bar: number;
  bas: number;
}

type FooReadonly = Readonly<Foo>; 

let foo:Foo = {bar: 123, bas: 456};
let fooReadonly:FooReadonly = {bar: 123, bas: 456};

foo.bar = 456; // Okay
fooReadonly.bar = 456; // ERROR: bar is readonly
```

#### 型推論
TypeScriptは、いくつかの簡単なルールに基づいて変数の型を推論(およびチェック)する  
1. 変数の型は、定義によって推論される  
2. 戻り値の型は、return文によって推測される
3. 構造化(オブジェクトリテラルの作成)の下でも機能する  
次のような場合、fooの型は`{a:number, b:number}`と推論される
```ts
let foo = {
    a: 123,
    b: 456
};
// foo.a = "hello"; // Would Error: cannot assign `string` to a `number`
```

#### 型の互換性
型の互換性は、あるものを別のものに割り当てることができるかどうかを決定する  
[型の互換性 doc](https://typescript-jp.gitbook.io/deep-dive/type-system/type-compatibility)  

#### never型
Never型=値を持たない型  
特徴  
- 値を持たない
- どんな変数も入れることが出来ない(Never型は値を持たないから)
- どんな型にも入れることができる(Never型は値を持たないので、空集合のようなもの)  

never型の使い道配下の二つを満たす時
- 実行される可能性のあるreturn文が存在しないと判断できる
- この関数は最後まで到達することはないと判断できる

voidとの違い  
- Void型は関数が正常に終了した結果何も返さない。
- Never型は、そもそも関数が正常に終了して値が帰ってくるわけない

参考資料[TypeScriptのNever型とは？](https://qiita.com/macololidoll/items/1c948c1f1acb4db6459e)  
