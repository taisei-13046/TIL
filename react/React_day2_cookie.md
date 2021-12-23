# やったこと
### cookie, CSRF対策について

今日本当はカスタムフックについてまとめようと思っていたが、DjangoでTwitter cloneを作成する途中でcookieの内容が出てきてわからなかったので、まとめることにした。
対象となるコードは以下のコードである。  
[Django Doc クロスサイトリクエストフォージェリ (CSRF) 対策](https://docs.djangoproject.com/ja/1.11/ref/csrf/)
```
    function getCookie(name) {
        var cookieValue = null;
        if (document.cookie && document.cookie != '') {
            var cookies = document.cookie.split(';');
            for (var i = 0; i < cookies.length; i++) {
                var cookie = jQuery.trim(cookies[i]);
                // Does this cookie string begin with the name we want?
                if (cookie.substring(0, name.length + 1) == (name + '=')) {
                    cookieValue = decodeURIComponent(cookie.substring(name.length + 1));
                    break;
                }
            }
        }
        return cookieValue;
    }
    var csrftoken = getCookie('csrftoken');

    function csrfSafeMethod(method) {
        // these HTTP methods do not require CSRF protection
        return (/^(GET|HEAD|OPTIONS|TRACE)$/.test(method));
    }
    $.ajaxSetup({
        crossDomain: false, // obviates need for sameOrigin test
        beforeSend: function (xhr, settings) {
            if (!csrfSafeMethod(settings.type)) {
                xhr.setRequestHeader("X-CSRFToken", csrftoken);
            }
        }
    });

```
正直初見では何が書いてあるのかさっぱりわからなかったので、噛み砕いて理解していいく。

### 1. cookieとは何か
Cookieは**WebサイトやWebサーバーにアクセスした人の情報を、ブラウザに一時的に保存するための仕組み**  
Cookieが有効化されると、使用中のブラウザで初めてアクセスしたWebサイトに、Webサイト側が指定した訪問ユーザーを識別できる情報が保存される。  
1st party Cookie と 3rd party Cookieがある
> - 1st party Cookie：実際に訪問したウェブサイトのドメインから発行される。ウェブサイトごとに独自で発行されているため、複数のウェブサイトの横断はできない。また、同じウェブサイトを再訪する場合でも、ブラウザや端末が違えば過去の情報の呼び出しは不可能。
> - 3rd party Cookie：訪問したウェブサイトのドメインではなく、ウェブサイト上に掲載されている広告配信事業社のドメインなどから発行される。

![スクリーンショット 2021-12-23 12 51 35](https://user-images.githubusercontent.com/78260526/147185482-8a00aac3-dd58-482f-bb9e-29d95592834a.png)
#### キャッシュとの違い
キャッシュは**一度表示したWebページの情報を一時的に保存する仕組み、または保存した一時データそのものを指す言葉**
- Cookie：ユーザーに関する情報を保存する
- キャッシュ：Webページそのもののデータを一時的に保存する

#### cookieを使う目的
1. 情報入力を省略できる
 同じWebサイトにアクセスするユーザーの負担を軽減する
2. 個々に適した広告表示
3. 行動データの収集

参考資料
[「Cookieとは？」わかりやすく解説｜仕組みや無効化・削除する方法](https://persol-tech-s.co.jp/hatalabo/it_engineer/567.html)

cookieについて理解が深まったところで、改めて上のコードの理解に移る。

ここでは何をしているのか。
大元の目的は**クロスサイトリクエストフォージェリ (CSRF) 対策**である

### 2. CSRFとは
掲示板や問い合わせフォームなどを処理するWebアプリケーションが、本来拒否すべき他サイトからのリクエストを受信し処理してしまう脆弱性を利用した攻撃手法。

この対策として、AJAXのPOSTにもCSRFトークンをデータに含めなくてはいけない。  
それには、**X-CSRFTokenという独自ヘッダーにCSRF トークンの値を設定すること**で解決する  
初めにトークンを取得する。
```
    function getCookie(name) {
        var cookieValue = null;
        if (document.cookie && document.cookie != '') {
            var cookies = document.cookie.split(';');
            for (var i = 0; i < cookies.length; i++) {
                var cookie = jQuery.trim(cookies[i]);
                // Does this cookie string begin with the name we want?
                if (cookie.substring(0, name.length + 1) == (name + '=')) {
                    cookieValue = decodeURIComponent(cookie.substring(name.length + 1));
                    break;
                }
            }
        }
        return cookieValue;
    }
    var csrftoken = getCookie('csrftoken');
```
- document.cookieで文書に関連付けられたクッキーを読み書きすることができる。
```
allCookies = document.cookie;
```
> 上記のコードで allCookies は、セミコロンで区切られたクッキーのリストです (つまり key=value のペア)。なお、それぞれの key および value の周囲にはホワイトスペース (空白やタブ文字) をおくことができます。実際のところ、 RFC 6265 ではそれぞれのセミコロンの後に空白1文字を入れることを必須としていますが、一部のユーザーエージェントはこれに従っていません。  
[MDN cookie](https://developer.mozilla.org/ja/docs/Web/API/Document/cookie)  

上記のコードでtokenを取得し、それをAJAXのrequestに設定する。
```
function csrfSafeMethod(method) {
    // these HTTP methods do not require CSRF protection
    return (/^(GET|HEAD|OPTIONS|TRACE)$/.test(method));
}
$.ajaxSetup({
    beforeSend: function(xhr, settings) {
        if (!csrfSafeMethod(settings.type) && !this.crossDomain) {
            xhr.setRequestHeader("X-CSRFToken", csrftoken);
        }
    }
});
```

ちょっとDjangoの内容が混じってしまったが、cookieとCSRF対策についての理解が深まった。

#### その他やったこと
- DDDを読む
