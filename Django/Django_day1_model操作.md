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

この部分大切そうだからもう少し深ぼる。。。  
null と blankの組み合わせは以下の4パターンがある  
- **blank=False, null=Falseのパターン**
    - フォームからの投稿時にこのフィールドの入力は必須、データベースに保存される値も空になってはいけない。
- **blank=True, null=Trueのパターン**
    - フォームから投稿するときにこのフィールドの入力は必須ではない、データベースに保存される値になにも入っていなくても大丈夫
- **blank=True, null=Falseのパターン**
    - フォームから投稿するときにはこのフィールドの入力は必須ではない、しかし、データベースに保存される値が空になってはいけない。
    - これが一番意味がわからないが、CharFieldやTextFieldのような文字列値においては、空の値はnullではなくて空文字列として保存されるため、文字列を扱うフィールドに限っては、blank=Trueのみの指定を許すことができる
- **blank=False, null=Trueのパターン**
    - フォームからの入力時には必ずこのフィールドを入力する必要がある、しかし、データベースの値は空であっても構わない
    - これは使う機会が少ない  
このようにnullとblankの組み合わせを場合分けしてみると、お互いの違いがよくわかる。
