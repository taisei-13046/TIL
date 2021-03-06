## やったこと
validationについて

## [クライアント側のフォームデータ検証](https://developer.mozilla.org/ja/docs/Learn/Forms/Form_validation)  

クライアント側検証は最初のチェックであり、ユーザーの使い勝手を良くするために重要な機能ですクライアント側で不当なデータを捕捉することで、ユーザーはすぐに修正できます。もしも無効なデータがサーバーに送られてから拒否される場合、サーバーへの往復とクライアント側に戻ってユーザーにデータを修正するように指示することにより、かなり時間を浪費します。  

しかし、クライアント側の検証はセキュリティ対策とは考えられません!アプリは常にサーバー側でもクライアント側と同様に送信されたデータのセキュリティをチェックします。なぜならクライアント側の検証は容易に回避することができて、悪意のユーザーは簡単に、サーバーへ不正なデータを送信できます。  

フォームを検証する必要がある理由
1. 正しいデータを正しい書式で入力してほしい。
2. ユーザーのデータを保護したい。
3. 自分たちを守りたい。

### 内蔵フォーム検証の利用
- `required`: 属性は値が入力されているべきかどうかを指定します。
- `minlength 属性と maxlength属性`: データ長の最小値と最大値を指定します。
- `min 属性と max属性`: 値の最小値と最大値を指定します。
- `type 属性`: その入力データが数値や、E メールアドレスや、特定の指定型かを指定します。
- `pattern 属性`: データが指定された正規表現にマッチするかどうかを指定します。

#### HTML 属性: required
[HTML 属性: required](https://developer.mozilla.org/ja/docs/Web/HTML/Attributes/required)  

論理属性の required 属性は、存在する場合、所有するフォームを送信する前にユーザーが入力に値を指定しなければならないことを示します。 required 属性は text, search, url, tel, email, password, date, month, week, time, datetime-local, number, checkbox, radio, file の `<input>` 型と `<select>` および `<textarea>` のフォームコントロール要素で対応しています。これらの入力型や要素の何れかに設定された場合、 :required 擬似クラスが一致します。属性が設定されていない場合は :optional 擬似クラスが一致します。

この属性は range と color は対応していませんし、どちらも既定値を持っているので関係がありません。 hidden は、非表示のフォームにユーザーが記入することを期待できないため、対応していません。また、 image を含むボタンの種類もいずれも対応していません。  

入力欄に required 属性がある場合、 :required 擬似クラスも適用されます。逆に、 required 属性に対応していて、この属性が設定されていない入力欄は、 :optional 擬似クラスに一致します。  

**属性の相互作用**  
読み取り専用フィールドは値を持つことができないので、 required は readonly 属性が指定されている入力欄には影響を与えません。  

**ユーザビリティ**  
required属性を設定する、その `<input>, <select>, <textarea>` が必須であることをユーザーに知らせるために、コントロールの近くに目に見える表示を提供してください。さらに、必須フォームコントロールを :required 擬似クラスでターゲットにし、必須であることを示すようにスタイル付けしてください。これにより、視覚障碍者のユーザーのユーザービリティが向上します。しかし、 aria-required="true" を追加しても、ブラウザーと読み上げソフトの組み合わせがまだ required に対応していない場合には問題ありません。   

**アクセシビリティの考慮**  
ユーザーにフォームコントロールが必須であることを知らせる表示を提供してください。色盲、認知機能の違い、スクリーンリーダーを使用しているかどうかにかかわらず、すべてのユーザーが要件を理解できるように、メッセージを伝えるものがテキスト、色、マーキング、属性などの多面的なものであることを確認してください。

#### HTML attribute: pattern
[HTML attribute: pattern](https://developer.mozilla.org/ja/docs/Web/HTML/Attributes/pattern)  

pattern 属性は、フォームコントロールの値が一致すべき正規表現を指定します。 null 以外の値が pattern 値によって設定された制約に適合しない場合、 ValidityState オブジェクトの読み取り専用の patternMismatch プロパティが真になります。  

pattern属性は、制約検証を通過させるために、入力の value が一致するべき正規表現です。これは有効な JavaScript の正規表現でなければならず、 RegExp 型で使用されたり、 正規表現ガイドで説明されているものと同じものです。通り正規表現をコンパイルする際に 'u' フラグを指定することで、パターンが ASCII ではなく Unicode コードポイントの並びとして扱われるようになります。パターンテキストの周りには、スラッシュを指定してはいけません。


pattern 属性に対応している入力型の中には、特に email および url 入力型のように、一致しなければならない期待値の構文を持っているものがあります。 pattern 属性が存在せず、値がその値型の期待される構文と一致しない場合、 ValidityState オブジェクトの読み取り専用の typeMismatch プロパティが真になります。  

**ユーザービリティ**  
pattern を含める場合は、コントロールの近くの可視テキストでパターンについて説明してください。さらに、パターンを説明する title 属性を含めてください。ユーザーエージェントは、制約検証中に title の内容を使用して、パターンが一致しないことをユーザーに伝えることができます。ブラウザーによっては、タイトルの内容を持つツールチップを表示し、視覚障碍者のユーザーの使い勝手を向上させます。さらに、支援技術は、コントロールにフォーカスを合わせるとタイトルを声に出して読むことができる場合がありますが、アクセシビリティのためにはこれに頼るべきではありません。  

**アクセシビリティの考慮**  
コントロールに pattern 属性がある場合、 title 属性が使われていれば、そのパターンを説明しなければなりません。テキストコンテンツを視覚的な表示するために title 属性に頼ることは、多くのユーザーエージェントがアクセス可能な方法で属性を公開しないので、一般的には推奨されません。ブラウザーによっては、タイトルを持つ要素の上にポインターを置いたときにツールチップを表示しますが、キーボードのみのユーザーやタッチのみのユーザーは除外されてしまいます。これが、どのようにコントロールに記入すれば要件に合うかをユーザーに知らせる情報を含める必要がある理由の一つです。

一部のブラウザーでは title を使用してエラーメッセージを表示していますが、ブラウザーはポインターを置いたときにもタイトルをテキストとして表示することがあり、エラーが発生していない状況でも表示されることがあるので、エラーが発生したかのような言葉を使用しないように注意してください。

要素が妥当な場合は、次のようになります。

- 要素が CSS の :valid 疑似クラスに一致します。これにより、妥当な要素に特定のスタイルを適用することができます。
- ユーザーがデータを送信しようとすると、ブラウザーは止めるものが他になければ（JavaScript など）、フォームを送信します。

要素が不正なときは、次のようになります。

- 要素が CSS の :invalid 疑似クラスに一致します。これにより、不正な要素に特定のスタイルを適用することができます。
- ユーザーがデータを送信しようとすると、ブラウザーはフォームをブロックしてエラーメッセージを表示します。

#### required属性
required 属性は、使うのがもっとも簡単な HTML5 の検証機能です。入力欄を必須にしたい場合は、この属性を使用して要素をマークすることができます。この属性が設定されていて、要素が :required にマッチすると、UI疑似クラスとフォームは送信されず、入力が空の場合のエラーメッセージが表示されるでしょう。空のままでは、この入力は不正とみなされ、:invalid 疑似クラスにマッチします。

### JavaScript を使用したフォーム検証
#### HTML5 の制約検証 API
多くのブラウザーが 制約検証API  に対応しています。この API は各フォーム要素で使用できる一連のメソッドやプロパティで構成されています。

- HTMLButtonElement (`<button>` 要素を表現)
- HTMLFieldSetElement (`<fieldset>` 要素を表現)
- HTMLInputElement ( `<input>` 要素を表現)
- HTMLOutputElement (`<output>` 要素を表現)
- HTMLSelectElement (`<select>` 要素を表現)
- HTMLTextAreaElement (`<textarea>` 要素を表現)

制約検証 API には、上記の要素で利用できる、次のプロパティがあります。

- validationMessage: コントロールが合格していない制約検証 (もしあれば) を説明するローカライズ済みのメッセージです。またはコントロールが制約の検証の対象ではない場合 (willValidate が false) または要素の値が制約に合格している場合は、空文字列です。
- validity: 要素の検証状態を説明する ValidityState オブジェクトです。取りうる検証状態の詳細は ValidityStateのリファレンスを参照してください。下記はよく使われるものを少し、一覧にしています:
- patternMismatch: 値が指定した patternにマッチしない場合 true を、マッチする場合 false を返す。true なら、要素は :invalid CSS 擬似クラスにマッチする。
- tooLong: maxlength 属性で指定した最大値より値が長い場合 true を、同じ長さ以下の場合 false を返す。true なら、要素は :invalid CSS 擬似クラスにマッチする。
- tooShort: minlength 属性で指定した最小値より値が短い場合 true を、同じ長さ以上の場合false を返す。true なら、要素は :invalid CSS 擬似クラスにマッチする。
- rangeOverflow: max 属性で指定し最大値より値が大きい場合true を、同じ大きさ以下の場合 false を返す。true なら、要素は :invalid と :out-of-rangeCSS 擬似クラスにマッチする。
- rangeUnderflow: min 属性で指定し最小値より値が小さい場合true を、同じ大きさ以上の場合 false を返す。true なら、要素は :invalid と :out-of-rangeCSS 擬似クラスにマッチする。
- typeMismatch: 値が要求する文法でない場合 (type が email か url のとき)は true を、文法が正しい場合は false を返す。true なら、要素は :invalid CSS 擬似クラスにマッチする。
- valid: 要素が検証制約をすべて満たす、ゆえに妥当とみなされる場合true を、いずれかの制約を満たさない場合 false を返す。true なら、要素は :valid CSS 擬似クラスにマッチする。そうでない場合は :invalid CSS 擬似クラスにマッチする。
- valueMissing: 要素に required 属性があって値がない場合は true を、そうでない場合 false を返す。true なら、要素は :invalid CSS 擬似クラスにマッチする。
- willValidate: フォームが送信されるときに要素が検証される場合に true を返します。そうでない場合は false を返します。

















