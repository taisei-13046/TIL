## やったこと
Reactの環境構築の続き!

### Issue Templateについて
Issueのtemplateを作成したい場合には、二つの方法がある。  
一つはSettingsから設定する方法  
もう一つは`.github/workflows/ISSYE_TEMPLATE`に設定する方法  

#### 設定する内容: 
1. バグの詳細
2. 報告者が想定する原因
3. 発生状況を再現
4. 端末情報
5. 二次障害の懸念
を記載する


[リポジトリ用に Issue テンプレートを設定する](https://docs.github.com/ja/communities/using-templates-to-encourage-useful-issues-and-pull-requests/configuring-issue-templates-for-your-repository)  


## ESLintについて
ESLint設定しておきながらあんまり知らないな〜と思ったので調べてみる  
今回、"eslint:recommended"を指定したが実際中で何をチェックしているのか知らなかった  

### "eslint:recommended"は何をチェックしているのか
この記事を参照 [ESLintの推奨設定（eslint:recommended）のチェック内容](https://www.tam-tam.co.jp/tipsnote/javascript/post11934.html)  
[List of available rules - ESLint - Pluggable JavaScript linter](https://eslint.org/docs/rules/)  

1. `no-cond-assign`  
if, for, while, and do...whileの条件式で代入演算子（=など）を許可しない。  
比較演算子（==,===など）のタイプミスで代入演算子が入力された箇所などを見つけてくれる  

2. `no-console`  
consoleオブジェクトのメソッド（console.log等）の記述を許可しない。  
公開時の消し忘れを防いでくれる。  

3. `no-constant-condition`  
if, for, while, and do...whileや三項演算子（?:）の条件式を定数式（リテラルなど）にすることを許可しない。  
必ず実行されたり、一度も実行されなかったりするなど、条件文として不適切な記述が無いかチェックしてくれる  

4. `no-control-regex`  
正規表現内でASCII制御文字の使用を許可しない。   

5. `no-debugger`  
開発時のブレークポイントの設定などに使用するdebuggerステートメントの記述を許可しない。  
公開時の消し忘れを防いでくれる。

6. `no-dupe-args`  
関数宣言や関数式の引数の重複を許可しない。  

7. `no-dupe-keys`  
オブジェクトリテラルでキーの重複を許可しない。  

8. `no-duplicate-case`  
switch文でcaseの重複を許可しない。

9. `no-empty-character-class`  
正規表現で空の文字クラス（[]）を許可しない。

10. `no-empty`  
空のブロックを許可しない。

11. `no-ex-assign`  
try...catch 文でcatchブロック内で例外オブジェクトを上書きすることを許可しない。
```js
try {
    // code
} catch (e) {
    e = 10; // 例外オブジェクトを上書き
}
```

12. `no-extra-boolean-cast`  
不必要なboolean型の型変換を許可しない。

13. `no-extra-semi`  
不必要なセミコロンを許可しない。

14. `no-func-assign`  
関数宣言として記述された関数への上書きを許可しない。

15. `no-inner-declarations`  
ネストブロック内での変数や関数宣言を許可しない。 （デフォルトでは関数宣言のみ）
```js
if (test) {
    function doSomething() { }
}
 
function doSomethingElse() {
    if (test) {
        function doAnotherThing() { }
    }
}
```

16. `no-invalid-regexp`  
RegExp コンストラクタ内で無効な正規表現文字列を許可しない。

17. `no-irregular-whitespace`  
Unicodeの特殊な空白文字を許可しない。

18. `no-obj-calls`  
グローバルオブジェクトを関数として呼び出すことを許可しない。

19. `no-regex-spaces`  
正規表現文字列の中で２文字以上の連続した空白を許可しない。

20. `no-sparse-arrays`  
空のスロットを含む配列を許可しない。  
連続したカンマはカンマのタイプミスである可能性がある。  

一通り見たけど、割と当たり前な部分をチェックしてくれてるんだな〜〜

### "react/recommended"は何をチェックしているのか？
[eslint-plugin-react](https://github.com/yannickcr/eslint-plugin-react#list-of-supported-rules)  

1. `react/display-name`  
Prevent missing displayName in a React component definition  
(Reactコンポーネント定義でdisplayNameが欠落しないようにする)  
＊これはクラスコンポーネントの際に必要になるため割愛  

2. `react/no-children-prop`  
Prevent passing of children as props.  
(childrenをpropsとして渡すことを禁止する)  

incorrect  
```tsx
<div children='Children' />

<MyComponent children={<AnotherComponent />} />
<MyComponent children={['Child 1', 'Child 2']} />

React.createElement("div", { children: 'Children' })
```
correct
```tsx
<div>Children</div>

<MyComponent>Children</MyComponent>

<MyComponent>
  <span>Child 1</span>
  <span>Child 2</span>
</MyComponent>

React.createElement("div", {}, 'Children')
React.createElement("div", 'Child 1', 'Child 2')
```

3. `react/no-danger-with-children`  
Report when a DOM element is using both children and dangerouslySetInnerHTML  
DOM 要素が children と dangerouslySetInnerHTML の両方を使用している場合reportする  

#### dangerouslySetInnerHTMLとは

マークダウンエディタを作りたい場合など、ブラウザ上で記述したHTMLのプレビューを作りたいときに使う  
しかし、XSSになる危険性があるのであまり使う機会は少ないかも  

参考記事 [ReactのdangerouslySetInnerHTML使ってみた](https://qiita.com/hiromoon/items/f3ed77abd338139ba97b)  

4. `react/no-deprecated`	
非推奨のメソッドを使用しないようにする  
incorrect
```tsx
// old lifecycles (since React 16.9)
componentWillMount() { }
componentWillReceiveProps() { }
componentWillUpdate() { }
```
correct
```tsx
UNSAFE_componentWillMount() { }
UNSAFE_componentWillReceiveProps() { }
UNSAFE_componentWillUpdate() { }
```

5. `react/no-direct-mutation-state`  
Prevent direct mutation of this.state  
this.stateの直接的な変更を防ぐ  

6. `react/no-find-dom-node`  
Prevent usage of findDOMNode  
findDOMNodeの使用を
h失せ具







