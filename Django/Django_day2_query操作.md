## やったこと
今日は昨日に引き続き、Djangoのdocumentを読もうと思っていたが、  
最終課題でmiharunさんから**SQLクエリのN+1問題**の指摘をいただいたので、それについて調べる。  
( ちょっと調べてみたけど、やばいむずい...)

### SQLクエリのN+1問題
SQLクエリのN+1問題とは、
- **一覧に表示するデータを取得するため**に SELECT を 1 回実行（N レコード返される）
- **各データの関連データを取得するため**に SELECT を N 回実行
- データベースアクセス（SELECT）が合計 N+1 回も実行されるというもの

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

