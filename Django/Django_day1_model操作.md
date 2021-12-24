# やったこと
Modelの操作をいまいち理解せずにしている気がするので、改めて振り返りとしてまとめてみる。  
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


## フィールドオプション
1. **null**: True の場合、Django はデータベース内に NULL として空の値を保持する
2. **blank**: True の場合、フィールドはブランクになることが許容される
3. **choices**: デフォルトのフォームウィジェットが標準のテキストボックスではなくセレクトボックスになり、与えられた選択肢を選ぶように制限される
4. **default**: そのフィールドのデフォルト値。
5. **primary_key**: True の場合、設定したフィールドはそのモデルの主キーとなる。設定されなかったら、自動的に設定される。
6. **unique**: True の場合、そのフィールドはテーブル上で一意となる制約を受ける。


### null と blankの違い  
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
参考資料[nullとblankの違いについて](https://djangobrothers.com/blogs/django_null_blank/#blankFalsenullTrue)

### choicesについて
- フィールドを選択肢として使用するには、2タプルの sequence を使用
```
YEAR_IN_SCHOOL_CHOICES = [
    ('FR', 'Freshman'),
    ('SO', 'Sophomore'),
    ('JR', 'Junior'),
    ('SR', 'Senior'),
    ('GR', 'Graduate'),
]
```
- choices の順番を変更すると、変更のたびに新しいマイグレーションが生成される  

### 詳細な (verbose) フィールド名
```
first_name = models.CharField("person's first name", max_length=30)
```
上のように第一引数にverbose_nameを指定できる。  
verbose_nameとは、**管理画面でのモデルの名前を指定することができるもの**であり、指定しないとモデル名を解体した文字列がそのまま管理画面に表示される。

## リレーション 
### 多対一 (many-to-one) 関係
ForignKeyを使用する
```
class Car(models.Model):
    company_that_makes_it = models.ForeignKey(
        Manufacturer,
        on_delete=models.CASCADE,
    )
    # ...
```
- フィールド名はモデル名を小文字にしたものをおすすめ

### 多対多 (many-to-many) 関係
```
from django.db import models

class Topping(models.Model):
    # ...
    pass

class Pizza(models.Model):
    # ...
    toppings = models.ManyToManyField(Topping)
```
- 命名は関係モデルオブジェクトの複数形が推奨
- ManyToManyField インスタンスは、フォームで編集されるオブジェクトの側にある
- 上の例では、 toppings は Pizza の中にあります  
    ( Topping が pizzas の ManyToManyField を持つとするよりはそのほうがいいでしょう ) 。  
    というのは、ピザが複数のトッピングを持つほうが、トッピングが複数のピザの上にあるというよりも自然だからです。  
    このようにすることで、ユーザは `` Pizza`` フォームでトッピングを選ぶことができるようになります。  

### 多対多リレーションにおける追加フィールド
ピザとトッピングを組み合わせる程度の多対多リレーションを扱うのであれば、標準の ManyToManyField で十分。  
しかし、2 つのモデルのリレーションに他のデータを付加したくなることもある。  
このような場合、 Django ではそのような多対多リレーションを規定するのに使われるモデルを指定することができる。  
そうすることで、中間モデルに追加のフィールドを配置することができます。  
中間モデルは、 through 引数で中間として振る舞うモデルを指定することで、 ManyToManyField に紐付けることができる。  
```
from django.db import models

class Person(models.Model):
    name = models.CharField(max_length=128)

    def __str__(self):
        return self.name

class Group(models.Model):
    name = models.CharField(max_length=128)
    members = models.ManyToManyField(Person, through='Membership')

    def __str__(self):
        return self.name

class Membership(models.Model):
    person = models.ForeignKey(Person, on_delete=models.CASCADE)
    group = models.ForeignKey(Group, on_delete=models.CASCADE)
    date_joined = models.DateField()
    invite_reason = models.CharField(max_length=64)
```
### 一対一 (one-to-one) 関係
OneToOneFieldを使用する
[一対一モデル使用例](https://docs.djangoproject.com/ja/4.0/topics/db/examples/one_to_one/)

## モデルの継承
