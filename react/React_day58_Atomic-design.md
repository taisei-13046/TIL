## やったこと
Atomic Designについて

## Atomic Design
[Atomic Design における Atom, Molecule, Organism の見極め方](https://a-suenami.hatenablog.com/entry/2019/04/29/173415)  

### Atoms
> Atoms are the basic building blocks of matter. Applied to web interfaces, atoms are our HTML tags, such as a form label, an input or a button.

基本的にはHTMLタグ要素になるレベルで分ける  


### Molecules
> Molecules are groups of atoms bonded together and are the smallest fundamental units of a compound. These molecules take on their own properties and serve as the backbone of our design systems.

「**それ自体が独立して存在はできないが、Atom ほど無機質でなく、最低限の意味をそれ自体が帯びる要素**」  

たとえば「削除ボタン」は「ボタン」ほど汎用的ではないが、それが何かを削除するものであることをユーザに伝えることしかできず、それ単体では存在できない。つまり、最低限の意味を持った上での汎用化と言える。


### Organisms
> Organisms are groups of molecules joined together to form a relatively complex, distinct section of an interface.

UI設計、ビジュアルデザイン、HTMLコーディングを実際に行う場合に「〇〇エリア」「〇〇ボックス」「〇〇領域」と呼ばれるもので、それ自体が独立して意味を持ち、ユーザとのインタラクションを定義可能なものという位置付けになる。  

Organism は groups of molecules と定義されているが、実際には直下に Atom を直接持ってもいいし、Organism の中に Organism を持つ、つまり Organism がネストされることもある。  
















