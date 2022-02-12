## やったこと
最終課題のレビュー(Login, Logout)をまとめる  

いろんな方針があるが、今回は一例  

### Login, Logoutにはdjangoに搭載された`django.contrib.auth`を使用する

これを使用してurls.pyをかくと以下のようになる  

```python
from django.urls import path, include

from . import views

urlpatterns = [
    path('', include('django.contrib.auth.urls')),
]
```

すると以下の機能が追加される  
[django/django/contrib/auth/urls.py](https://github.com/django/django/blob/main/django/contrib/auth/urls.py)  
```python
accounts/login/ [name='login']
accounts/logout/ [name='logout']
accounts/password_change/ [name='password_change']
accounts/password_change/done/ [name='password_change_done']
accounts/password_reset/ [name='password_reset']
accounts/password_reset/done/ [name='password_reset_done']
accounts/reset/<uidb64>/<token>/ [name='password_reset_confirm']
accounts/reset/done/ [name='password_reset_complete']
```

#### Loginの実装内容
[django/django/contrib/auth/views.py#login](https://github.com/django/django/blob/main/django/contrib/auth/views.py#L42)  

codeを見ると、get_default_redirect_urlで以下のようにdefaultの遷移先を取得している  
```python
def get_default_redirect_url(self):
    """Return the default redirect URL."""
    return resolve_url(self.next_page or settings.LOGIN_REDIRECT_URL)
```

[resolve_url](https://github.com/django/django/blob/3702819227fd0cdd9b581cd99e11d1561d51cbeb/django/shortcuts.py#L117)  

そのため、まずはsettings.pyに`LOGIN_REDIRECT_URL`を指定することが必要  

[django/django/contrib/auth/forms.py](https://github.com/django/django/blob/3702819227fd0cdd9b581cd99e11d1561d51cbeb/django/contrib/auth/forms.py#L174)  

Formを見ると、usename, passwordで認証が行われていることがわかる  
＊　この認証方式は変更可能

ちなみにログインの実装は、これだけで済む!(超簡単)  
```html
<h2>Login</h2>
<form method="post">
  {% csrf_token %}
  {{ form.as_p }}
  <button type="submit">Login</button>
</form>
```

##### ちなみに`form.as_p`って何？？
[form.as_pの意味を分かりやすく解説【ソースコード付き】](https://codor.co.jp/django/meaning-form-as-p)  
**form.as_pは「formの内容をpタグで囲って表示」という意味**  

```html
<p>
  <form action="/your-name/" method="post">
   <label for="your_name">Your name: </label>
   <input id="your_name" type="text" name="your_name" value="{{ current_name }}">
   <input type="submit" value="OK">
  </form>
</p>
```
というhtmlの形に変換される  




















