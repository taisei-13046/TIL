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

すると以下のpathが設置される  
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



















