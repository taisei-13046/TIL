# やったこと
Djangoでviewをいじってる時に、Modelの操作をいまいち理解せずにしている気がするので、振り返りとしてまとめてみる。  
まずはドキュメントを読む [Django Doc Model](https://docs.djangoproject.com/ja/4.0/topics/db/models/)
## Modelについて

```
from django.db import models

class Person(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)
```
このモデルを定義した場合は、
```
CREATE TABLE myapp_person (
    "id" serial NOT NULL PRIMARY KEY,
    "first_name" varchar(30) NOT NULL,
    "last_name" varchar(30) NOT NULL
);
```
このようなデータベースのテーブルを作成する。 (SQLがさっぱりなのでまた読まないとな。。。)

#### フィールドオプション
1. **null**: True の場合、Django はデータベース内に NULL として空の値を保持する
2. **blank**: True の場合、フィールドはブランクになることが許容される
null と blankの違い  
- blank がバリデーション由来である一方、 null は完全にデータベース由来である。
- blankはDjangoのフォームからの投稿が空かどうかを判定するもの、nullはデータベースの中身が空かどうかを判定するもの
