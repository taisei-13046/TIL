## やったこと
git rebaseと styled-componentsについて

### git rebaseについて
[公式DOC](https://git-scm.com/book/ja/v2/Git-%E3%81%AE%E3%83%96%E3%83%A9%E3%83%B3%E3%83%81%E6%A9%9F%E8%83%BD-%E3%83%AA%E3%83%99%E3%83%BC%E3%82%B9)  
[Git rebase](https://www.atlassian.com/ja/git/tutorials/rewriting-history/git-rebase)  
Git には、あるブランチの変更を別のブランチに統合するための方法が大きく分けて二つあります。 **merge** と **rebase** です。  
rebase は、1 つのブランチから別のブランチへの変更を統合することを専門とした、2 つの Git ユーティリティの 1 つです。もう 1 つの変更統合ユーティリティは git merge です。マージは常に、変更レコードを先へ動かします。代わりに、rebase には強力な履歴書き換え機能を備えています。  

コンテンツという観点において、リベースは、あるコミットから他のコミットにブランチを移動させ、別のコミットからブランチを作成したように見せています。  
しかし Git の内部では、**新たなコミットを生成してそれを移動先のベースコミットに適用する**ことによってこれを行なっています。  
ここでは、**ブランチそのものは同じものに見えていても、それを構成するコミットは全く異なる**ことを理解することが重要です。   

#### Git リベース標準モードと Git リベースのインタラクティブ モード
Git rebase interactive は、git rebase が -- i 引数を受け入れます。これは、「インタラクティブ」を表します。引数が指定されていない場合、コマンドは標準モードで実行します。  
Git rebase 標準モードは、現在の作業ブランチのコミットを自動的に取得し、渡されたブランチの HEAD に適用します。  
git rebase を -i フラグで実行するとインタラクティブなリベース セッションが開始されます。インタラクティブなリベースでは、すべてのコミットをそのまま新しい ベースに移動するのではなく、対象となる個々のコミットの改変が可能です。これを使用して、既存の一連のコミットの削除、分割、改変を行って履歴を整理することができます。これはちょうど、`Git commit --amend` の強化版と言えます。  

##### `git commit --amend`
git commit --amend コマンドは、直前のコミットを変更する最も便利な方法です。これを使用することで全く新しいコミットを作成する代わりに、ステージングされた変更を前のコミットと組み合わせることができます。また、スナップショットを変更せずに、前のコミット メッセージを単純に変更する際にも使用されます。しかし、改変では、直前のコミットが変更されるだけでなく、全体が置き換えられます。つまり、修正されたコミットは独自の ref を持つ新しいエンティティとなります。Git にとっては全く新しいコミットのように見えます (以下の図ではアスタリスク (`*`) を使用して視覚化されています)。  

![スクリーンショット 2022-02-09 14 45 32](https://user-images.githubusercontent.com/78260526/153129423-89830fa1-9f34-4be4-bd6b-eaade9852ae2.png)  

レビューを行うには、git commit --amend を使用すると、直前のコミットへ移動して、そこに新しいステージングされた変更を追加します。  

インタラクティブなリベースセッションを使用して現在のブランチを  にリベースするコマンドです。このコマンドを実行すると、エディターが開き、リベースする個々のコミットに対するコマンド (下で説明します) の入力が可能となります。ここでのコマンドは、個々のコミットを新しいベースに移動する方法を指定します。また、エディターにおけるコミットの並びを直接編集することによりコミットの順番を並び替えることもできます。  

インタラクティブなリベースの真価は、書き換えられた main ブランチの履歴で確認できます。事情を知らない者にとっては、開発者が有能で最小限のコミットを一度ずつ実行しただけで開発を完了できたかのように見えます。  

#### rebase の高度な用途
```
git rebase --onto <newbase> <oldbase>
```
--onto 特定の ref を rebase の最後に渡せる、より強力なフォームまたは rebase を実現します。  
```
 o---o---o---o---o  main
      \
       o---o---o---o---o  featureA
            \
             o---o---o  featureB
```
このようなリポジトリの場合
featureB は featureA に基づいていますが featureA の変更に依存しておらず、main からの分岐に過ぎないことがわかります。  
```
git rebase --onto main featureA featureB
```
このコマンドを実行すると、このようになる
```
                  o---o---o  featureB
                 /
o---o---o---o---o  main
 \
  o---o---o---o---o  featureA
```

#### リベースの危険性を理解する
Git リベースを操作する際に考慮する必要がある注意点の 1 つとして、rebase ワークフロー中にマージの競合の頻度が上がる可能性があるということがあります。これは、main から離れた古いブランチがある場合に発生します。最終的には、main に対して rebase を実行します。その際に、ブランチの変更と競合する可能性がある多くの新しいコミットが含まれる場合があります。main に対してブランチのリベースを頻繁に行ってより頻繁にコミットを行うことによって、この問題は簡単に修正できます。  
rebase のより深刻な注意点は、インタラクティブな履歴書き換えからコミットが失われることです。インタラクティブ モードで rebase を実行し、squash または drop などのサブコマンドを実行すると、ブランチの直前のログからコミットが削除されます。一見すると、これによって、コミットが完全に削除されたように見えます。git reflog を使用すると、これらのコミットが復元され、rebase 全体を元に戻すことができます。  
Git rebase 自体はそれほど危険ではありません。本当の危険は、履歴を書き換えるインタラクティブな rebase を実行し、他のユーザーと共有しているリモート ブランチへ結果を強制プッシュした際に発生します。この操作には、プルした際に他のリモート ユーザーの作業が上書きされる機能を持っているため、回避すべきパターンです。  

### git reflog
Reflogs は、ローカル リポジトリでいつ Git refs が更新されたかを記録します。Git stash 内には、ブランチ tip reflog 以外に、特別な reflog が格納されています。Reflogs は、ローカル リポジトリの .git ディレクトリ内に保存されています。  
```
 git reflog show --all 
```
このコマンドはすべての ref の完全な reflog を取得できます。  

## styled-componentsの公式DOCについて
### useTheme
useThemeを使うことによって、styled-components外からでもthemeを参照することができる  
```tsx
import { useTheme } from 'styled-components'

const MyComponent = () => {
  const theme = useTheme()

  console.log('Current theme: ', theme)
  // ...
}
```

[React Styled Components — Hooks, Refs, and Security](https://levelup.gitconnected.com/react-styled-components-hooks-refs-and-security-281fb8ab0341)  
### refs
styled-componentsにもDOM操作をするrefを渡すことが可能  
```tsx
import React from "react";
import styled from "styled-components";
const Input = styled.input`
  padding: 0.5em;
  margin: 0.5em;
  background: greenyellow;
  border: none;
  border-radius: 3px;
`;
export default function App() {
  const inputRef = React.useRef();
  React.useEffect(() => {
    inputRef.current.focus();
  }, []);
  return (
    <div>
      <Input ref={inputRef} />
    </div>
  );
}
```
 ()
### Security
styled-componentsでは、任意の入力を補間として使用できるため、その入力をサニタイズするように注意する必要があります。ユーザーの入力をスタイルとして使用すると、ユーザーのブラウザで評価されるあらゆるCSSが、攻撃者がアプリケーションに配置できるようになる可能性があります。  

この例では、不適切なユーザー入力が、ユーザーの代わりにAPIエンドポイントを呼び出すことにさえつながることを示しています。  

```tsx
// Oh no! The user has given us a bad URL!
const userInput = '/api/withdraw-funds'

const ArbitraryComponent = styled.div`
  background: url(${userInput});
  /* More styles here... */
`
```

非常に注意してください! これは明らかにでっち上げの例ですが、CSS インジェクションは目に見えないところで悪い影響を与えることがあります。IEのいくつかのバージョンでは、URL宣言の中で任意のJavaScriptを実行することさえあります。

JavaScriptからCSSをサニタイズするために、CSS.escapeという規格が近々発表されます。これはまだブラウザ間であまりサポートされていないので、Mathias Bynensによるポリフィルをアプリで使用することをお勧めします。



