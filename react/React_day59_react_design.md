## やったこと
Reactの設計思想について

## SOLID原則
[The S.O.L.I.D Principles in Pictures](https://medium.com/backticks-tildes/the-s-o-l-i-d-principles-in-pictures-b34ce2f1e898)  
[イラストで理解するSOLID原則](https://qiita.com/baby-degu/items/d058a62f145235a0f007)  

### S — Single Responsibility(単一責任の原則)

![スクリーンショット 2022-02-23 11 12 33](https://user-images.githubusercontent.com/78260526/155251033-e23e0e13-7c5d-4de5-9878-bb5202356ebd.png)  

A class should have a single responsibility
クラスは、単一の責任を持つべきだ。


Goal  
This principle aims to separate behaviours so that if bugs arise as a result of your change, it won’t affect other unrelated behaviours.

### O — Open-Closed(オープン・クローズドの原則)  

![スクリーンショット 2022-02-23 12 07 56](https://user-images.githubusercontent.com/78260526/155255583-51bdf052-446b-46e8-9a94-baf76adfb932.png)  

Classes should be open for extension, but closed for modification  
クラスは、拡張にはオープンで、変更にはクローズドであるべきだ。

この原則は、クラスの既存の動作を変更することなく、クラスの動作を拡張することを目的としています。これは、そのクラスが使用されている場所でバグが発生するのを避けるためです。


### L — Liskov Substitution(リスコフの置換原則)  

![スクリーンショット 2022-02-23 16 17 21](https://user-images.githubusercontent.com/78260526/155275747-8fb9060b-b0f7-49d1-9c30-66eaf2d75c24.png)  

If S is a subtype of T, then objects of type T in a program may be replaced with objects of type S without altering any of the desirable properties of that program.
SがTのサブタイプである場合、プログラム内のT型のオブジェクトをS型のオブジェクトに置き換えても、そのプログラムの特性は何も変わらない。  

この原則は、親クラスやその子クラスがエラーなしで同じ方法で使用できるように、一貫性を保つことを目的としています。


### I — Interface Segregation(インターフェイス分離の原則)

![スクリーンショット 2022-02-23 16 21 22](https://user-images.githubusercontent.com/78260526/155276139-e792a6e1-43ef-401b-afb7-8355dc2c7b93.png)

Clients should not be forced to depend on methods that they do not use.  

クライアントが使用しないメソッドへの依存を、強制すべきではない。  
この原則は、動作のセットをより小さく分割して、クラスが必要なもののみを実行することを目的としています。


### D (Dependency Inversion)　依存性逆転の原則

![スクリーンショット 2022-02-23 16 23 04](https://user-images.githubusercontent.com/78260526/155276302-4c2f1724-1f65-4825-81dc-cc1fea3370ca.png)  

- 上位モジュールは、下位モジュールに依存してはならない。どちらも抽象化に依存すべきだ。
- 抽象化は詳細に依存してはならない。詳細が抽象化に依存すべきだ。

この原則は、インターフェイスを導入することにより、上位レベルのクラスが下位レベルのクラスに依存するのを減らすことを目的としています。  




