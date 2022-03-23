## やったこと
Falsy値についての面白い記事を見つけたので、自分なりにまとめる


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








