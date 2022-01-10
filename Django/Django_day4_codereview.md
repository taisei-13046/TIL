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



参考資料
- [コードレビュー虎の巻](https://qiita.com/teradonburi/items/2fa475c860d0fb16c0eb)
- [Googleに学ぶコードレビューのポイント](https://cloudsmith.co.jp/blog/efficient/2021/08/1866630.html)
