## やったこと
今日は昨日に引き続き、Djangoのdocumentを読もうと思っていたが、  
最終課題でmiharunさんから**SQLクエリのN+1問題**の指摘をいただいたので、それについて調べる。  
( ちょっと調べてみたけど、やばいむずい...)

### SQLクエリのN+1問題
SQLクエリのN+1問題とは、
- **一覧に表示するデータを取得するため**に SELECT を 1 回実行（N レコード返される）
- **各データの関連データを取得するため**に SELECT を N 回実行
- データベースアクセス（SELECT）が合計 N+1 回も実行されるというもの (**JOIN して 1 回の SQL で取得した方が効率的**)

このようなモデルがあったとき、
```python
class Table1(models.Model):
    text = models.TextField()

class Table2(models.Model):
    table1 = models.ForeignKey(Table1, on_delete=models.CASCADE)
```
Table2 の全レコードが参照している Table1 オブジェクトのカラムを出力する場合、
```python
for table2 in Table2.objects.all():
    print(table2.table1.text)
```
この処理をする際に実行されるSQLは：
1. Table2 の全レコードを取得するSQLを一回
2. Table1 を取得するSQLを Table2 のレコード数分。  
このように合計で**N+1回**SQLが実行されてしまっている。

### まずはDjnagoでN+1問題を発見する
今回は[この記事](https://selfs-ryo.com/detail/django_nplusone)を参照して解決した。  
N+1問題のありかを発見するためには、**django-debug-toolbar**をインストールする。  
そして各種設定を行うと、  
![スクリーンショット 2021-12-25 14 08 46](https://user-images.githubusercontent.com/78260526/147377951-17f604cd-ea70-4825-a434-567d9876101a.png)
このようなツールバーが右に表示されるため、そのSQLをみるとN+１の存在が発見できる。  
今回の場合は4つの重複したクエリが発行されてしまっていることがわかる。  

### N+1問題の解決方法
結論, [select_related()](https://docs.djangoproject.com/ja/3.1/ref/models/querysets/#select-related)を使用することで解決できる  
ではselect_relatedとはなんなのか？
##### select_related: クエリを実行したときに、指定された外部キーのオブジェクトも一緒にとってくるというもの
select_relatedを使用しない
```python
b = Entry.objects.get(id=4)  # DBを叩く
p = b.author         # DBを叩く
c = p.hometown       # DBを叩く
```
使用すると、
```python
# 一度のクエリでauthorとhometownテーブルからもオブジェクトを取得する
b = Entry.objects.select_related('author__hometown').get(id=4)
p = b.author         # 取得済みのオブジェクトを参照
c = p.hometown       #取得済みのオブジェクトを参照
```
これによって、無駄にSQLを叩く回数が減った。

では実際に先ほどのdjango-debug-toolbarで確認してみる。
![スクリーンショット 2021-12-25 14 09 20](https://user-images.githubusercontent.com/78260526/147378093-4ff3729c-622b-40f0-aa67-f26c2c857630.png)
すると、先ほどの状態に比べて、重複していたクエリが解消されていることがわかる  
また、SQL文を確認すると、テーブルがJOINされて発行されるクエリが1つになっている。  
クエリが発行される時間も2.75 -> 1.62に短くなっている。  
これからはN+1問題にも意識しながらクエリ発行をしていきたい。

参考資料
[DjangoのN+1問題を考える](https://selfs-ryo.com/detail/django_nplusone)
[document](https://docs.djangoproject.com/ja/3.1/ref/models/querysets/#select-related)
[N+1問題におけるORMの重たさについて](https://aish.dev/misc/orm_n1problem.html)


## クエリに関するDjangoのdocument
### オブジェクトを作成する
オブジェクトを生成するためには、作成するモデルのクラスにキーワード引数を渡してインスタンス化し、  
そのデータをデータベースに保存するために save() を呼び出す。  
[save source code](https://github.com/django/django/blob/main/django/db/models/base.py)  
- 内部でINSERTSQL分が実行されている
- saveを呼ぶまではDBに保存されない
- saveは値を返さない
- オブジェクトの作成と保存を一つの処理で行うには、 create() メソッドを利用する

### オブジェクトを取得する
データベースからオブジェクトを取得するには、モデルクラスの Manager から QuerySet を作る
#### [Manager](https://docs.djangoproject.com/ja/4.0/topics/db/managers/#django.db.models.Manager)とは
- Django のモデルに対するデータベースクエリの操作を提供するインターフェイス
- Django アプリケーション内の1つのモデルに対して、Manager は最低でも1つは存在する
- デフォルトでは、Django は **objects** という名前の Manager を各 Django モデルクラスに対して追加している
- Manager はモデルのインスタンスでなく、モデルのクラスを経由してのみアクセスできる
    - それは "テーブル水準" の処理と "レコード水準" の処理とで責任を明確に分離するためです。

#### すべてのオブジェクトを取得する
```python
all_entries = Entry.objects.all()
```

#### フィルタを用いて特定のオブジェクトを取得する
1. **フィルターのチェーン**
```python
>>> Entry.objects.filter(
...     headline__startswith='What'
... ).exclude(
...     pub_date__gte=datetime.date.today()
... ).filter(
...     pub_date__gte=datetime.date(2005, 1, 30)
... )
```
このようにフィルターをチェーン状につなげることができる

2. **フィルターを適用した QuerySet はユニーク**  
QuerySet に対して絞り込みを適用するごとに、前の QuerySet から独立した完全に新しい QuerySet が作られる  
そのため絞り込みごとに独立した QuerySet が作られるため、保存したり何度も再利用したりできる
```python
>>> q1 = Entry.objects.filter(headline__startswith="What")
>>> q2 = q1.exclude(pub_date__gte=datetime.date.today())
>>> q3 = q1.filter(pub_date__gte=datetime.date.today())
```
これら３つは全て独立している。

3. **QuerySet は遅延評価される**  
 QuerySet を作る行為はいかなるデータベース操作も引き起こさない

#### get() を用いて1つのオブジェクトを取得する
filter() は、たとえクエリーにマッチしたのが1つのオブジェクトだけだったとしても、常に QuerySet を返す。  
仮にクエリーにマッチするのが1つだけだとわかっていたら、get()を使用するのが良い
```python
>>> one_entry = Entry.objects.get(pk=1)
```
- クエリにマッチする結果が存在しない場合、 get() は DoesNotExist 例外を起こす
- 同様に get() のクエリーが2つ以上のアイテムにマッチした場合にもMultipleObjectsReturned 例外が起こる

#### QuerySet の要素数を制限する
Python のリストスライスのサブセットを使うことで QuerySet の結果を特定の要素数に制限することができる
```python
>>> Entry.objects.all()[:5]

>>> Entry.objects.all()[5:10]
```
ただし負のインデックスには対応していない (例: Entry.objects.all()[-1])

#### フィールドルックアップ
**SQL の WHERE 句の内容を指定する手段**
```python
>>> Entry.objects.filter(pub_date__lte='2006-01-01')
```
このコードは、SQL文に直すと、
```SQL
SELECT * FROM blog_entry WHERE pub_date <= '2006-01-01';
```
に変換される。  
- ルックアップに指定するフィールドはモデルが持つフィールド名でなければいけない。  
    - ただし1つだけ例外があり、 ForeignKey の場合にはフィールド名の末尾に **_id** を付けた名前を指定することができる
- 無効なキーワード引数を指定すると、ルックアップ関数は TypeError を起こす

いくつかのルックアップ
**exact, iexact, contains**
```python
>>> Entry.objects.get(headline__exact="Cat bites dog")
>>> Blog.objects.get(name__iexact="beatles blog")
>>> Entry.objects.get(headline__contains='Lennon')
```
