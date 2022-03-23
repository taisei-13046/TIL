## やったこと
Falsy値についての面白い記事を見つけたので、自分なりにまとめる
[Falsy値を比較せずにそのまま判定に使うことはやめよう](https://zenn.dev/okunokentaro/articles/01fynkwmrkrbyzxgexvhv0hnez)  



### Falsy 値とは
[MDN Falsy](https://developer.mozilla.org/ja/docs/Glossary/Falsy)  

偽値 (falsy または falsey な値) は、 **Booleanコンテキスト**に現れたときに偽とみなされる値です。

![スクリーンショット 2022-03-23 17 38 14](https://user-images.githubusercontent.com/78260526/159657648-6ff9214a-34d0-46ad-b9e9-a48bd13e6cba.png)  

### Boolean コンテキストとは
[wiki booleanコンテキスト](https://ja.wikibooks.org/wiki/JavaScript/Boolean#%E3%83%96%E3%83%BC%E3%83%AA%E3%82%A2%E3%83%B3%E3%82%B3%E3%83%B3%E3%83%86%E3%82%AD%E3%82%B9%E3%83%88)  

if文の条件式に渡されたオブジェクトは、自動的に真偽値に変換されます。このように、暗黙に真理値に変換される文脈は、ブーリアンコンテキストとよばれます。

### Falsy 値は実際どう評価されているか
[ECMAScript if statement](https://tc39.es/ecma262/#sec-if-statement) 

```js
if ( Expression ) Statement else Statement
```

1. Let exprRef be the result of evaluating Expression.
2. Let exprValue be ToBoolean(? GetValue(exprRef)).
3. If exprValue is true, then
a. Let stmtCompletion be the result of evaluating the first Statement.
4. Else,
a. Let stmtCompletion be the result of evaluating the second Statement.
5. Return ? UpdateEmpty(stmtCompletion, undefined).

ToBoolean()という箇所で「その値が Falsy 値なのかどうか」という関心が作用する

### ToBoolean 抽象操作
[ECMAScript ToBoolean](https://tc39.es/ecma262/#sec-toboolean)  

抽象操作は訳語であり、原典では"Abstract Operations"と記載されている  

これは、JavaScript の標準的な関数による様々な「型変換」について変換の仕様を定めている  

1997 年当時の仕様書では、この節は"Abstract Operations"とはなっておらず、単に"Type Conversion"とされていました。初版には次のように記載されています。

> The ECMAScript runtime system performs automatic type conversion as needed

最新の ECMAScript の仕様では次のように記載されています。

> The ECMAScript language implicitly performs automatic type conversion as needed

> 最新版では"implicitly"（暗黙的）のニュアンスが追記されていることがわかります。そう、TypeScript による explicitly（明示的）な型付けが進んでいるこの時代において、TypeScript が JavaScript に変換されて動作する以上「TypeScript を利用する explicitly を心がける開発者」であっても、言語仕様として implicitly な型変換が行われる可能性を常に考慮せざるを得ない

Booleanコンテキストとは「ToBoolean 抽象操作が行われる状況」のことを指す  

### Falsy値を比較せずにそのまま判定に使うことはやめる
「false以外の Falsy 値を Boolean コンテキストに渡すことをやめよう」と言い換えることができる  

```js
function getLabel(name: string): string {
  if (!name) {
    return "名無しさん";
  }
  return `${name}さん`;
}
```

```js
function getLabel(name: string): string {
  if (!name.length) {
    return "名無しさん";
  }
  return `${name}さん`;
}
```

上記に二つの例がある  

前者!nameと後者!name.lengthは同じ結果となりますが、Boolean コンテキストの解釈が異なります。nameはstring型の引数であるため、 ToBoolean 抽象操作にてfalseとなりうるのは""（空文字列）のときだけです。  

そして、name.lengthという式にした場合、lengthはnumber型の値を返すため（根拠1, 2）number 型の値が ToBoolean 抽象操作によってfalse となりうるのは0, -0, NaNのときであることから、!name.lengthについては"".lengthの値が0になるためにfalseとなります。  

```ts
function getLabel(name: string): string {
  if (name === "") {
    return "名無しさん";
  }
  return `${name}さん`;
}
```

そのため、上のように書いていく方針がいい

Boolean コンテキストを成す ToBoolean 抽象操作は、あらゆる値をtrueかfalseに「暗黙的に」変換しています。  

### 筆者の結論
「TypeScript は暗黙的な型変換を避けるようなデザインに寄っているのだから、ToBoolean 抽象操作についてエラーとならなかったとしても言語の設計意図を汲み、Boolean コンテキストにおいてはfalse以外の Falsy 値をそのまま渡すべきではない」






