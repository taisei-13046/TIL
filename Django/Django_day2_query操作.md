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
```
class Table1(models.Model):
    text = models.TextField()

class Table2(models.Model):
    table1 = models.ForeignKey(Table1, on_delete=models.CASCADE)
```
Table2 の全レコードが参照している Table1 オブジェクトのカラムを出力する場合、
```
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
