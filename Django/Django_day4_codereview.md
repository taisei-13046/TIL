## やったこと
フロントの最終課題を進める。ハッカソンの準備をする。  

## コードレビューについて
コース課題が終了してコードレビューをする立場になったが、なかなかどうしていいかわからないので調べる  


#### 「最悪を最初に」を基本としてレビューすべき
仕様やアルゴリズムに欠陥があるのに、typoにこだわってもしょうがないので、なにが最悪かを考え、それを防ぐための物からレビューをする  
- バグや設計変更があった時に、大変になるところを重点的に見る
- **モデル**の修正は大変なので念入りに見る
- セキュリティの問題を抱えやすい場所は注意する
  - ユーザー入力を埋め込んでる場所など (XSS/SQLインジェクション etc)

#### コードレビューの目的
コードベースの一貫性と保守可能性を維持すること


#### コードレビューのコツ
- **1行ずつ見る**  
- いいところは褒める

以下は最終課題においてレビューするべきである項目をまとめる
## SignUp機能
- **コードの最終行に改行があるかないか**
  - pythonでは問題はないが、C言語などでは「文字＋改行」で構成されている「行」が正しく認識されないことがある
  - エディタの設定を勧める
- .gitignoreにdb.sqlite3や__pychache__、仮想環境のフォルダを追加する
- testの書き方について
  - テストはユーザの行動に合わせてかく(Viewに合わせる)
  - [Django test code](https://github.com/django/django/blob/main/tests/auth_tests/test_views.py#L100)
- 余分な余白がないか（インデント、空白、空行）
- 命名規則が正しいか
  - [pep8 命名規則について](https://pep8-ja.readthedocs.io/ja/latest/#id24)
  - クラスはCapWords方式
  - 関数、変数は小文字で`_`でつなぐ
- Djangoの公式コーディングスタイルに則っているか
  - [django coding style](https://docs.djangoproject.com/ja/3.2/internals/contributing/writing-code/coding-style/)
  - クラス間は2行空ける

### coding style
#### importの順番
```python
# future
from __future__ import unicode_literals

# standard library
import json
from itertools import chain

# third-party
import bcrypt

# Django
from django.http import Http404
from django.http.response import (
    Http404, HttpResponse, HttpResponseNotAllowed, StreamingHttpResponse,
    cookie,
)

# local Django
from .models import LogEntry

# try/except
try:
    import yaml
except ImportError:
    yaml = None
```

#### Template style
```python
{{ foo }}
```
`{{foo}}`ではだめ

View style
(正解)
```python
def my_view(request, foo):
    # ...
```
(間違い)
```python
def my_view(req, foo):
    # ...
```
requestで呼ぶべき

#### Model style
- model名はスネークケース `first_name`  
- class Metaの上には１行改行

modelの順番
```python
All database fields
Custom manager attributes
class Meta
def __str__()
def save()
def get_absolute_url()
Any custom methods
```


### testについて
テストはユーザの行動に合わせて書くのが良い  
1. `https://domain/signup`を訪問する。
2. Formに名前とパスワードを入力して送信し、リダイレクトされる。
上記の２段階を経てユーザは会員登録をするが、これらはSignUpViewによって制御されている  
つまり、機能ごとにテストを書く時にはViewに合わせて書くのが良い  

その他Tips
#### 命名について
`successed_signup`よりも`signup_done`、`signup_complet`eや`signup_successed`が良い  
理由は以下のように機能ごとに並ぶから
```python
password_change
password_change_confirm
password_change_done
signup
signup_confirm
signup_done
```

#### importの順番
```python
from django.contrib import admin
from django.contrib.auth.admin import UserAdmin
from .models import Account
```
Django自体からimportする場合は1行空行を入れる

## PEP8を読む
### インデント
- 1レベルインデントするごとに、スペースを4つ使う!!  
- 行を継続する場合は、折り返された要素を縦に揃えるようにすべき
```python
# 正しい:

# 開き括弧に揃える
foo = long_function_name(var_one, var_two,
                         var_three, var_four)

# 引数とそれ以外を区別するため、スペースを4つ(インデントをさらに)加える
def long_function_name(
        var_one, var_two, var_three,
        var_four):
    print(var_one)

# 突き出しインデントはインデントのレベルを深くする
foo = long_function_name(
    var_one, var_two,
    var_three, var_four)
```

### タブか、スペースか?
スペースが好ましいインデントの方法  
タブを使うのは、既にタブでインデントされているコードと一貫性を保つためだけ  
Python では、インデントにタブとスペースを混ぜることを禁止している

### 1行の長さ
すべての行の長さを、最大**79文字**までに制限する  
(docstring やコメントのように) 構造に関する制約が少ないテキストのブロックについては、1行72文字までに制限すべき  

### 空行
- トップレベルの関数やクラスは、2行ずつ空けて定義
- クラス内部では、1行ずつ空けてメソッドを定義

### import
import文は、通常は行を分けるべき
```python
# 正しい:
import os
import sys

# 悪い:
import sys, os
```
import文 は次の順番でグループ化すべきです:

1. 標準ライブラリ
2. サードパーティに関連するもの
3. ローカルな アプリケーション/ライブラリ に特有のもの

そしてそれぞれには１行空白を入れるべき

**ワイルドカードを使った import (from `<module>` import *) は避けるべき**  
なぜなら、どの名前が名前空間に存在しているかをわかりにくくし、コードの読み手や多くのツールを混乱させるから  

### 式や文中の空白文字
余計な空白文字を使うのはやめましょう  
括弧やブラケット、波括弧 のはじめの直後と、終わりの直前:

```python
# 正しい:
spam(ham[1], {eggs: 2})
# 間違い:
spam( ham[ 1 ], { eggs: 2 } )
```
末尾のカンマと、その後に続く閉じカッコの間:

```python
# 正しい:
foo = (0,)
# 間違い:
bar = (0, )
```

### コメント
コードと矛盾するコメントは、コメントしないことよりタチが悪い  
コメントは複数の完全な文で書くべき  
はじめの単語はそれが小文字で始まる識別子でない限り、大文字にすべき  
あなたのコードが、自分の言葉を話さない人に 120% 読まれないと確信していなければ、コメントを**英語**で書く  

ブロックコメントは、一般的にその後に続くいくつか（またはすべて）のコードに適用され、そのコードと同じレベルにインデントされる  
ブロックコメントの各行は (コメント内でインデントされたテキストでない限り) # とスペースひとつではじまる  

### 命名規約
単一の文字 'l' (小文字のエル)、'O' (大文字のオー)、'I'(大文字のアイ) を決して変数に使わない  
フォントによっては、これらの文字は数字の1や0と区別が付かない  

モジュールの名前は、全て小文字の短い名前にすべき

関数の名前は小文字のみにすべき



参考資料
- [コードレビュー虎の巻](https://qiita.com/teradonburi/items/2fa475c860d0fb16c0eb)
- [Googleに学ぶコードレビューのポイント](https://cloudsmith.co.jp/blog/efficient/2021/08/1866630.html)
