## やったこと
ひたすら最終課題に取り組んだ。  


Djangoの最終課題のAjaxをFetchに書き換える。  
その中で詰まった部分をまとめる。。  

### 1. 途中からブラウザに何も反応しなくなった。。
これで実に3時間くらい無駄なことをしていた。。。  

#### 対処法
chromeのキャッシュを消去したら治った！！  
原因は掴めていないが、同様の現象が起きたときに対応できそう  


### 2. ひたすら403errorが出ていた！！
Djnagoはheaderに`'X-CSRFToken': csrftoken,`を設定しないと受け付けてくれないようだ  


### その他の変更点としてprefetch_relatedを使う
**一度のクエリでリレーション先のオブジェクトも取ってくるもの**  

#### select_related()との違い
select_related()の場合: リレーション先のオブジェクトも含めてSQLでSELECTを実行してオブジェクトを取得している  
そのためリレーション先のオブジェクトも含めて一度のクエリですべての対象オブジェクトを取得している  
しかし、これをM2Mの関係で行うとすごい数のオブジェクトを取得することになるため、  
select_relatedは`ForeignKey`か`one-to-one`のリレーションにだけ行うよう制限されてる  

一方で、prefetch_related()は: 対多のあギブキーに対してクエリの最適化を行える
リレーション先を取得するために別のクエリを発行する  
そして取得したオブジェクトとリレーション先をSQLではなく、Pythonを使ってJoiningする  

今回の自分のコードは
```python
context['post_list'] = Post.objects.prefetch_related('like_post').all()
```
このような感じで、受け取り先でもlike_post以上絞り込みをしないため、このままの記述で問題ない。  
しかし、  
```python
article = Article.objects.prefetch_related('comments').get(id=5)
comments = article.comments.filter(created__gte='2020-10-01').order_by('-created')
```
この記述のように、prefetch_relatedをした後にさらにfilterで絞り込みをしてしまうと、  
またさらにクエリが飛んでしまうことになる。  
この時には、追加のクエリを一つ飛ばしてさらに多くのデータを一緒に取得する方法を使うのが良い  
```python
article = Article.objects.prefetch_related(Prefetch('comments', queryset=Comment.objects.select_related('user').filter(created__gte='2020-10-01').order_by('-created'), to_attr='article_comments')).get(id=5)
comments = article.article_comments

for comment in comments:
    print(comment.user.username)
```
テンプレ  
`refetch_related(Prefetch(*related_name, queryset=*クエリ文, to_attr=*参照するための名前を設定))`  


参考資料  
- [prefetch_related()をちょっと詳しく調べてみた](https://mkai.hateblo.jp/entry/2018/11/05/234611)
- [select_relatedとprefetch_relatedでクエリの最適化](https://just-python.com/document/django/orm-query/select_related-prefetch_related)
