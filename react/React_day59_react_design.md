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





