## やったこと
sass記法について復習, コンテンツセキュリティポリシー (CSP), React Hook Form4


## React Hook Formについて
### register
[register](https://react-hook-form.com/api/useform/register)  
特に注目した点

#### valueAsNumber
valueAsNumberをつけないと、全てstringで扱われてしまう
```tsx
<input
  type="number"
  {...register("test", {
    valueAsNumber: true,
  })}
/>
```

`valueAsDate`, `setValueAs`なども同様  

#### errorの設定方法
```tsx
<input
  {...register("test", {
    required: 'error message' // JS only: <p>error message</p> TS only support string
  })}
/>
```
以下の書き方も可能
```tsx
<input
  {...register("test", {
    required: {
      value: ture,
      message: 'error message' // JS only: <p>error message</p> TS only support string
     }
  })}
/>
```

#### 自由なvalidationをかける
`validate`  
```tsx
<input
  {...register("test", {
    validate: value => value === '1' || 'error message'  // JS only: <p>error message</p> TS only support string
  })}
/>
// object of callback functions
<input
  {...register("test1", {
    validate: {
      positive: v => parseInt(v) > 0 || 'should be greater than 0',
      lessThanTen: v => parseInt(v) < 10 || 'should be lower than 10',
      // you can do asynchronous validation as well
      checkUrl: async () => await fetch() || 'error message',  // JS only: <p>error message</p> TS only support string
      messages: v => !v && ['test', 'test2']
    }
  })}
/>
```

#### isDirtyって何？
ていうかdirtyって何？w  

[What is "Dirty" in React.js?](https://stackoverflow.com/questions/35262546/what-is-dirty-in-react-js)  

> Dirty data - the data, that have been changed recently and DOM haven't been re-rendered according to this changes yet. So dirty checking is diff between next state and current state. 

最近変更されたデータであり、DOMはまだその変更に従って再レンダリングされていません。つまり、ダーティチェックとは、次の状態と現在の状態の差分である。  


[Reactjsのチュートリアルを読んで浮かんだ疑問とそれに対する答え](https://qiita.com/yuku_t/items/f453839377317e013537)  

あるcomponentのstateが setState() によって変更されると、Reactjsはそのcomponentにdirtyフラグを立てます。  

![スクリーンショット 2022-02-21 15 21 37](https://user-images.githubusercontent.com/78260526/154900057-912712fe-2ba7-4ba7-8376-6c6a2fb9be21.png)


そして処理が終わった段階でdirtyなcomponentとその子供達を全てrenderします。  

![スクリーンショット 2022-02-21 15 21 50](https://user-images.githubusercontent.com/78260526/154900073-938f682f-1480-4399-b3c8-49b8c66608a3.png)  

こうすることで新しい仮想DOMを構築するコストを極限まで減らしています。また仮想DOMの差分を計算する際も、変更されている可能性があるcomponentに絞って計算すればいいことになります。


### unregister
```tsx
import React, { useEffect } from "react";
import { useForm } from "react-hook-form";

interface IFormInputs {
  firstName: string;
  lastName?: string;
}

export default function App() {
  const { register, handleSubmit, unregister } = useForm<IFormInputs>();
  const onSubmit = (data: IFormInputs) => console.log(data);

  React.useEffect(() => {
    register("lastName");
  }, [register])

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <button type="button" onClick={() => unregister("lastName")}>unregister</button>
      <input type="submit" />
    </form>
  );
};
```

![スクリーンショット 2022-02-21 16 18 20](https://user-images.githubusercontent.com/78260526/154906905-7c262364-a7db-4182-b6d0-3e36c7e610d8.png)  

unregister後はregisterの関心から離れる  

### formState
#### isDirty
defaultValueと異なっていた場合Trueが返される






### Sass, SASS, SCSSの違い
[SassとSASSとSCSSの違いについて](https://uxmilk.jp/38084)  

##### Sass
CSSを拡張したメタ言語のこと

##### SASS
SASS記法やSASSファイルと書かれていることが多く、Sassの2つの記法のうちの一つ

##### SCSS
SCSS記法やSCSSファイルと書かれていることが多く、Sassの2つの記法のうちのよく使われている記法

最初に作られたのがSASS記法（.sass）で、後に作られたのがSCSS記法（.scss）になり、一般的にSCSS記法の方が広く普及しています。  

#### SASS
括弧ではなく、インデントで依存関係を示します。  
```sass
.article
  .title
    color: #222
```

#### SCSS
SCSS記法はネスト（入れ子）を主とした記法
```scss
div {
    margin: 0 auto;
    p {
        padding: 20px;
        span {
             font-color: red;
        }
    }
}
```

### cssの記法復習
[styled-components の :&>before(記号) まとめ](https://blog.ojisan.io/s-c-kigo/)  

#### hoge, fuga
`,` は複数のセレクタを対象にします。  

```css
h1,
h2 {
  color: red;
}
```

#### hoge fuga
スペースは子孫要素を表します。  

```css
div p {
  color: red;
}
```

#### hoge > fuga
`>` は直下要素を表します。

```css
div > p {
  color: red;
}
```

直下は 1 階層だけ下という意味  

```css
<div>
  <p>スタイル当たる</p>
  <form>
    <p>スタイル当たらない</p>
  </form>
</div>
```

#### hoge + fuga
`+` は直後の隣接要素を表します。  

```css
p + span {
  color: orange;
}
```

```html
<p class="hoge">
  一つ内側(p　.hoge)
  <span>二つ内側(span)</span>
</p>
<span>一つ内側(span)(ここにスタイルが当たる)</span>
<span>一つ内側(span)</span>
<span>一つ内側(span)</span>
```

#### hoge ~ fuga
`~` は 後続の隣接要素を表します。 `+` が直後だけだったのに対し、これは後続のもの全てが対象です。

```css
p ~ span {
  color: orange;
}
```

```html
<p class="hoge">
  一つ内側(p　.hoge)
  <span>二つ内側(span)</span>
</p>
<span>一つ内側(span)(ここにスタイルが当たる)</span>
<span>一つ内側(span)(ここにスタイルが当たる)</span>
<span>一つ内側(span)(ここにスタイルが当たる)</span>
```

#### hoge:hover
`:` は擬似クラス(pseudo-class)を表すのに使います。   
MDN の説明を借りるなら、擬似クラスは「**セレクターに付加するキーワードであり、選択された要素に対して特定の状態を指定します。**」です。  

#### hoge::before

`::` は擬似要素(Pseudo-elements)を表すのに使います。   
MDN の説明を借りるなら、擬似要素 Pseudo-elements は**セレクターに付加するキーワードで、選択された要素の特定の部分にスタイル付けできるようにするもの**です。  



## コンテンツセキュリティポリシー (CSP)
[コンテンツセキュリティポリシー (CSP)](https://developer.mozilla.org/ja/docs/Web/HTTP/CSP)  

コンテンツセキュリティポリシー (CSP) は、クロスサイトスクリプティング (XSS) やデータインジェクション攻撃などのような、特定の種類の攻撃を検知し、影響を軽減するために追加できるセキュリティレイヤーです。これらの攻撃はデータの窃取からサイトの改ざん、マルウェアの拡散に至るまで、様々な目的に用いられます。  

CSP を有効にするには、ウェブサーバーから Content-Security-Policy HTTP ヘッダーを返すように設定する必要があります   

CSP の第一の目的は XSS 攻撃の軽減と報告です。 XSS 攻撃とは、サーバーから取得したコンテンツをブラウザーが信頼する性質を悪用した攻撃です。ブラウザーはコンテンツの送信元を信頼するため、たとえ実際の送信元が見かけ上とは異なっていたとしても、悪意あるスクリプトが被害者のブラウザー上で実行されてしまいます。  

サーバー管理者が CSP を利用する場合、実行を許可するスクリプトの正しいドメインをブラウザーに向けて指定することにより、 XSS の発生する箇所を削減・根絶することができます。 CSP をサポートするブラウザーは、サーバーから指定された許可リストに載っているドメインのスクリプトのみ実行し、他のスクリプトはすべて無視します (インラインスクリプトや HTML 属性値のイベントハンドラも無視する対象に含まれます)。

コンテンツセキュリティポリシーを適用するには、該当するウェブページについて Content-Security-Policy HTTP ヘッダーを返すようにし、その値にはユーザエージェントに読み込ませたいリソースの情報を指定します。例えば、画像のアップロード・表示を行うページの場合、画像の出元は任意の場所で構いませんが、フォームの action 属性値は特定のエンドポイントに制限する必要があります。  


### [CSP(コンテンツセキュリティポリシー)について調べてみた](https://techblog.securesky-tech.com/entry/2020/05/21/)  
CSP(Content Security Policy)は、対応しているユーザーエージェント（通常はブラウザ）の挙動をWebサイト運営者が制御できるようにする宣言的なセキュリティの仕組み  
簡単に説明すると、XSS等を緩和する為、インラインスクリプトやevalなどを禁止したり、信頼できるコードなどを参照するように制限をかけたりするセキュリティの為のHTTPレスポンスヘッダーです。このHTTPレスポンスヘッダー(CSP)はAlexa Top 100万サイト中ではわずか4~6%しか利用されていません，裏を返せば約95%ぐらいのサイトはCSPをしていないということですね。単純に知名度がないのか他の理由があるのか気になりますが、ただ探してみると意外にもTwitterやpixiv、Facebookなどで使われていたりします。  


CSPの適応方法はHTTPレスポンスヘッダーに以下のように設定すれば使えます

```
Content-Security-Policy: policy
```

## OWASP
[Open Web Application Security Project Top 10（OWASP Top 10）](https://www.synopsys.com/ja-jp/glossary/what-is-owasp-top-10.html)  

### OWASPとは
Open Web Application Security Project（OWASP）は、ソフトウェアのセキュリティを向上させることを専門とした非営利団体です。OWASPは「オープン・コミュニティ」モデルの下で運営されており、誰でもプロジェクト、イベント、オンライン・チャットなどに参加して貢献することができます。OWASPの基本理念は、すべての資料と情報が無料で、誰でもWebサイトから簡単にアクセスできることです。OWASPは、ツール、ビデオ、フォーラム、プロジェクトからイベントまで、あらゆるものを提供します。つまりOWASPは、オープン・コミュニティの貢献者の幅広い知識と経験に裏打ちされた、汎用的なWebアプリケーション・セキュリティのリポジトリです  

### OWASP Top 10とは
OWASP Top 10は、Webアプリケーション・セキュリティに関する最も重大な10のリスクについてのランキングと修正のガイダンスを提供する、OWASPのWebサイトにあるオンライン・ドキュメントです。  

### 最新のOWASP Top 10
#### 1. インジェクション
コード・インジェクションは、攻撃者が無効なデータをWebアプリケーションに送信したときに発生します。攻撃者の意図は、アプリケーションに意図しない操作を実行させることです。  

#### 2. 認証の不備
特定のアプリケーションは、不適切に実装される場合が多くあります。具体的には、認証とセッション管理に関連する機能が正しく実装されていない場合、攻撃者はパスワード、キーワード、およびセッションを侵害できてしまいます。これにより、ユーザーIDなどが盗まれる可能性があります  

#### 3. 機密データの露出
機密データの露出とは、保存または送信された重要データ（社会保障番号など）が侵害された場合を指します。

#### 4. XML外部実態参照（XXE）
攻撃者は、脆弱なコンポーネント処理XMLを使用するWebアプリケーションを利用できます。攻撃者は、XMLをアップロードしたり、悪意のあるコマンドやコンテンツをXMLドキュメントに含めたりすることができます。

#### 5. アクセス制御の不備
アクセス制御の不備とは、攻撃者がユーザー・アカウントにアクセスできる場合を指します。攻撃者は、システムのユーザーまたは管理者として操作することができます。

#### 6. セキュリティ設定のミス
セキュリティの設定ミスとは、設計または構成の弱点が設定エラーまたは欠点に起因する場合を指します。

#### 7. クロスサイト・スクリプティング（XSS）
XSS攻撃は、アプリケーションにWebページ上の信頼できないデータが含まれている場合に発生します。攻撃者は、クライアント側のスクリプトをこのWebページにインジェクションします。

#### 8. 安全でないデシリアライゼーション
安全でないデシリアライゼーションは、デシリアライゼーションの欠陥により、攻撃者がシステム内のコードをリモートで実行できる脆弱性を指します。

#### 9. 機知の脆弱性を持つコンポーネントの使用
この脆弱性の名称はその性質を示しています。既知の脆弱性を含むコンポーネントを使用してアプリケーションを構築および実行するタイミングを表しています。

#### 10. 不十分なロギングと監視
ロギングと監視は、Webサイトの安全性を保証するために、Webサイトに対して頻繁に実行する必要のあるアクティビティです。サイトを適切にログに記録して監視しないと、サイトはより深刻な侵害アクティビティに対して脆弱になります。



















