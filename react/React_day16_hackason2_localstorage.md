## やったこと
hackasonの準備！！

### 型の拡張
```ts
type TPerson = {
  name: string;
  old: number;
};

type TPerson2 = TPerson & {
  address: string;
};
```
型を拡張する方法は`&`でつなぐ  

### リロードしてもデータを保存する
[リロードしてもデータを保持するためには「localStorage」](https://qiita.com/Ryusou/items/8bce84e7b036114b8d72)  
リロードしてもデータを保存するためには**localStorage**を使用すれば解決できる  
使い方は超簡単!  
```js
localStorage.setItem('myCat', 'Tom');

var cat = localStorage.getItem("myCat");
console.log(cat)
//出力結果　> "Tom"
```

### ただ、LocalStorageは使ってはいけないらしい
[HTML5のLocal Storageを使ってはいけない](https://techracho.bpsinc.jp/hachi8833/2019_10_09/80851)  

#### local storageとは
local storageはHTML5の新機能であり、Webブラウザ（すなわちブラウザのユーザー）がJavaScriptを用いて任意の情報を保存できる仕組み  

#### local storageの嬉しい点
- 「純粋なJavaScript」という点
  - local storageに代わる選択肢といえば現実にはcookieしかありませんが、  
    cookieの残念な点の1つは「Webサーバーが作成する必要がある」こと
- あらゆるWebサーバーから切り離された形でWebページを実行できるようになる
- 最大サイズが4 KBに制限されているcookieに比べてサイズの制約がかなり緩く、  
  主要なブラウザなら5 MBを超えるデータ・ストレージを楽々利用できる

#### local storageの残念な点
- local storageには「stringデータしか保存できない」
- local storageは「同期的」
  - つまり、local storageへの操作は同時に1つしか実行できない
  - 複雑なアプリケーションでlocal storageを使うと速度が落ちてしまい、使い物にない
- local storageは「web workersから利用できません」
- local storageに保存できるデータサイズには依然として上限がある
- local storageはあらゆるJavaScriptコードから自由にアクセスできてしまう
  - これはセキュリティ上非常に重大

local storageを使うべきシチュエーションは  
- 秘密情報を一切含まないこと
- 一般に入手可能な情報であること
- そこそこの量である（5 MBを超えない）こと
- stringのみの情報であること
- 保存するアプリケーションでパフォーマンスを要求されないこと

上の理由によって、local storageはセキュアではない！  

この世で最も危険な存在は**JavaScropt**である  
秘密情報をlocalStorgeに保存するということはこの世で最も危険なJavaScript様が史上最悪の地下金庫にあなたの大事な大事な秘密情報を保管しているのと本質的に変わらない  
この問題の行く先には、かの**クロスサイトスクリプティング攻撃（XSS）**が待ち構えている  
攻撃者があなたのWebサイトでJavaScriptを実行すると、local storageに保存されているデータを残らず取り出して、攻撃者が所有するドメインに送信できてしまう  

#### 重要なお知らせ: JSON Web Token（JWT）をlocal storageに保存してはいけない
セキュリティ上最大の脅威となるのはlocal storageにJWT（セッションデータ）を保存している人たち  
攻撃者がJWTをコピーすると、ユーザーがまったく気づかないうちにWebサイトへのリクエストをユーザーの代わりに実行できてしまう  

#### では、localStorageの代わりに何を使えばいいか
重要なデータを保存する必要が生じた場合は、以下のようにする  

>ユーザーがWebサイトにログインするときは、ユーザーのセッションIDを作成して、暗号化済み署名cookieに保存します。Webフレームワークを使っている方は、フレームワークガイドの「cookieでユーザーセッションを作成する方法」あたりの情報に従いましょう。
Webフレームワークで使うcookieライブラリは、種類を問わずhttpOnly cookieフラグを必ず設定してください。このフラグを設定することで、ブラウザ側で（訳注: JavaScriptを用いて）cookieを読み取ることは不可能になります。この設定は、cookieでサーバーサイドセッションを用いる場合の必須手順です。詳しくは勇敢なるJeff Atwoodの記事をご覧ください。

>使うcookieライブラリでは、SameSite=strict cookieフラグ（これはクロスサイトリクエストフォージェリ: CSRF 攻撃を防止します）と、secure=trueフラグ（cookieを暗号化済みコネクションでのみ設定できるようにします）も必ず設定してください

>ユーザーがサイトにリクエストを送信するたびに、必ずセッションID（セッションIDはユーザーから送信されたcookieから取り出します）を使ってアカウントの詳細情報をデータベースまたはキャッシュから取り出してください（どちらを使うかはサイトの規模によります）。

>ユーザーのアカウント情報を取り出して検証を終えたら、後はそこから関連する秘密情報を自由に取り出して利用します。

