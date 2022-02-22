## やったこと
Atomic Designについて

### styled-componentsをまとめる方法
[Multiple Inheritance (Styled Components)](https://stackoverflow.com/questions/61601451/multiple-inheritance-styled-components)  

```tsx
import styled, { css } from ‘styled-components’;

const baseStyles = css`
  background: #ddd
`;

const HeaderItem = styled(HorizontalListItem)`
  ${baseStyles}
`;

const HeaderDropDownLi = styled(DropDownLi)`   
  ${baseStyles}
`;
```
このようにすれば複数のcssを共通化することができる


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


## 食べログにおけるAtomic Design
[食べログでのAtomic Design 〜どう分類しているか編〜](https://note.com/tabelog_frontend/n/n4b8bcb44294c)  

### Organisms
#### 一般的な責務：

- サービスとして意味のある単位の塊。
- 他のAtoms/Molecules/Organismsや純粋なHTMLで構成される。
- 独立して成立するコンテンツを提供する。

#### 食べログでのOrganisms：

- ドメインが入ったらOrganisms。
- 他に依存するコンポーネントがなかったとしても、ドメインが入った時点でOrganismsにする。
- useContextによるContext接続可。
- **その機能のためのAPIを叩くのはここ。**

ドメインが入るとは？  
-> 特定のコンテンツ・コンテキストじゃないと使えない状態。

「他のコンポーネントに依存」とは？  
-> 別のコンポーネントをimportしている状態。

### Molecules
#### 一般的な責務：

- 一つ以上のAtomsに依存したcomponent。
- ユーティリティ的な塊

#### 食べログでのMolecules：

- 抽象的な機能を提供する。
- ドメインが入ってはいけない。
- 他のAtomsやMoleculesのコンポーネントに依存している。
- Contextへのアクセスはしない。
- 自分自身で状態はなるべく持たない。

**Atomsとの違いは「他のコンポーネント」に依存しているかどうか**  

### Atoms
#### 一般的な責務：

- これ以上分けられない塊。
- 汎用的に使えるcomponent。

#### 食べログでのAtoms：
- 抽象的な機能を提供する。
- ドメインが入ってはいけない。
- Contextへのアクセスはしない。
- 自分自身で状態はなるべく持たない。
- 他のコンポーネントに依存していなければAtoms。





