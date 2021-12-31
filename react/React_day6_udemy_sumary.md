## やったこと
Reactの復習 (Udemy じゃけえさんのやつ)

### memoで囲うことの意味
(前提)  
Reactでは親のコンポーネントが再レンダリングされた場合、その子コンポーネントの再レンダリングされてしまう。  
そこで、propsに変化がなければ再レンダリングをする必要が無いのが分かっている場合に、そのコンポーネントのレンダリングを抑制することで、パフォーマンスが向上する可能性がある。  
`React.memo`を使うことで、Propsが変更された時のみ再レンダリングされるようになる！  
複数のコンポーネントで成り立っているコンポーネントは基本**React.memo**を使用するのがいい  
しかし、闇雲にmemo化するのも違う気がしている  
メモ化する際は
1. コンポーネントの再レンダリングに時間がかかる
2. Propsが変わらない
3. 何度も再レンダリングされる
4. パフォーマンスに大きく影響を与えている  

これらに当てはまる際にReact.memoを使用するといい  
このサイトがとても面白かった!!  
[メモ化することへのコストについて](https://cam-inc.co.jp/p/techblog/530551646413914939)  

### コンポーネントにアロー関数をpropsとして渡す場合はuseCallbackを使う
(重要)*アロー関数はレンダリング時に再生成されるため、レンダリング前の関数とは別物として評価される*  
すると、渡された側のコンポーネントは異なるpropsが渡されたと認識し、再レンダリングが走ってしまう  

### default marginについて
`<p>`の場合デフォルトで上下にmarginが当たっている！！  
![スクリーンショット 2021-12-31 10 44 43](https://user-images.githubusercontent.com/78260526/147798225-b354cf09-bb32-4290-9eb7-d0d2b06e1fea.png)  
それを解除するために`margin: 0`を指定することで、上下のmarginがなくなる  
参考資料 [default marginを覚えておこう](http://msw316.jpn.org/hp_kouza/html215/dft_margin.html)  

### Atomic Designについて

#### Atoms
Atomic designで大切なのは、そのコンポーネントの**役割**を考えること  
例えば、 components/atoms/button/PrimaryButton.jsx の場合
```js
export const PrimaryButton = (props) => {
  const { children } = props;
  return <button>{children}</button>;
};
```
表示するボタンの文字をchildrenで受け取って、再利用できるようにする  


#### molecules


#### organisms


#### templates


#### pages


### styled-componentsについて
#### styled-componentsの要素をまとめてBase化して使用できる  
components/atoms/button/BaseButton.jsx
```js
import styled from "styled-components";

export const BaseButton = styled.button`
  color: #fff;
  padding: 6px 24px;
  border-radius: 9999px;
  border: none;
  outline: none;
  &:hover {
    cursor: pointer;
    opacity: 0.8;
  }
`;
```
components/atoms/button/PrimaryButton.jsx
```js
import styled from "styled-components";
import { BaseButton } from "./BaseButton";

export const PrimaryButton = (props) => {
  const { children } = props;
  return <SButton>{children}</SButton>;
};

const SButton = styled(BaseButton)`
  background-color: #40514e;
`;
```

#### styled-componentsでも子要素に対して、sassのようにネスとして記載できる  
```js
const SDl = styled.dl`
  text-align: left;
  dt {
    float: left;
  }

  dd {
    padding-left: 32px;
    padding-bottom: 8px;
    overflow-wrap: break-word;
  }
`
```


###  高さを指定する100vhと100%の違い
CSSで大きさを指定する際の単位  
- `px`：ピクセル。画素数
- `%`：親要素に対する割合
- `vh`：ビューポート（画面サイズ）の高さに対する割合。100vhは画面の高さと同じ（100%）を表す
- `vw`：ビューポートの幅に対する割合
- `vmin`：画面の高さか幅の小さい方に対する割合
- `vmax`：画面の高さか幅の大きい方に対する割合