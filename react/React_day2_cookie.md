# やったこと
### cookieについて

今日本当はカスタムフックについてまとめようと思っていたが、DjangoでTwitter cloneを作成する途中でcookieの内容が出てきてわからなかったので、まとめることにした。
対象となるコードは以下のコードである。
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
