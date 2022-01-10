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
