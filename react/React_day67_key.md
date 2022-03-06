## やったこと
reactのmapでkeyにindexを渡す危険性について

### Index as a key is an anti-pattern

[Index as a key is an anti-pattern](https://robinpokorny.medium.com/index-as-a-key-is-an-anti-pattern-e0349aece318)  

このサイトのdemoがめちゃくちゃわかりやすい！  
[DEMO](https://jsbin.com/wohima/edit?output)  

keyをindexにすると、一番初めの要素が`index = 0`つまり、`key = 0`になるが、その上に新規で要素を追加すると`key = 0`がその要素になってしまうため、意図したものと並び順が変わってしまう。










