## やったこと
reactのmapでkeyにindexを渡す危険性について

### Index as a key is an anti-pattern

[Index as a key is an anti-pattern](https://robinpokorny.medium.com/index-as-a-key-is-an-anti-pattern-e0349aece318)  

このサイトのdemoがめちゃくちゃわかりやすい！  
[DEMO](https://jsbin.com/wohima/edit?output)  

#### keyにindexを渡してもいい条件
1. 配列とその中の要素が静的(計算も変更もされない)
2. 配列の中の要素がidを持ってない
3. 配列がreorderやfilterされることが絶対ない

### useMemoとuseCallbackの違い






