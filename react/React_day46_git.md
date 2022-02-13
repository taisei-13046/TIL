## やったこと
Gitについて勉強!!!
超初心に戻ってgitを復習していく!!!
あとは、HTMLのアクセシビリティとセマンティクスについて

## レビューまとめ
### SVGについて
自分が作成したSVG要素がなぜか右によっていたり、右側が欠けていたりした。  
<img width="210" alt="スクリーンショット 2022-02-10 11 04 52" src="https://user-images.githubusercontent.com/78260526/153323184-a173f935-7272-42e4-aef5-abacda62dd25.png">    
原因: 
1. **svgを取得する際にviewboxSizeを考慮していないsvgをexportしてしまった**
2. **viewboxSizeの指定が20になっていた(本来24)**

### `const Svg = styled.svg``;`
Q. スタイルを指定していないstyled-componentsにはどんな意味があるのか？  
A. その要素の役割を示すのには使える！ ただ、上のは流石に意味ない  
  これはチームの方針による!


参考にしたサイト [Bitbucket Cloud での Git の使用方法](https://www.atlassian.com/ja/git/tutorials/what-is-version-control)  
### はじめに
### 新しいリポジトリの初期化: `git init`
新しいリポジトリを作成するには、git init コマンドを使用します。git init は、新しいリポジトリの初期セットアップ時に使用するワンタイム コマンドです。このコマンドを実行すると、現在の作業ディレクトリ内に新しい .git サブディレクトリが作成されます。これによって、新しい main ブランチも作成されます。  

##### git init 対 git clone
全体的に見れば、どちらも「新しい Git リポジトリの初期化」に使用できます。ただし、git clone は git init に依存しています。git clone は、既存のリポジトリのコピーを作成する際に使用します。内部では、git clone はまず git init を呼び出して新しいリポジトリを作成します。その後、既存のリポジトリからデータをコピーして、一連の作業ファイルを新たにチェック アウトします。  

#### ベアリポジトリ --- `git init --bare`
```
git init --bare <directory>
```

作業ディレクトリを持たない空の Git リポジトリを作成して初期化します。共有リポジトリは必ず --bare フラグを指定して作成する必要があります  
--bare フラグを指定して作成したリポジトリの名称の最後には、慣例的に .git が付加されます。たとえば、my-project という名称のリポジトリのベアバージョンは、my-project.git という名称のディレクトリに格納されます。  
git push と git pull は行うが、直接コミットを行わない場合は、ベアリポジトリを作成します。ノンベアリポジトリにブランチのプッシュを行うと変更が上書きされる可能性があるため、中央リポジトリは必ずベアリポジトリとして作成する必要があります。--bare は、リポジトリを開発環境ではなくストレージ領域として認識させる方法と考えてください。つまり、実質的にすべての Git ワークフローにおいて、中央リポジトリはベアであり、開発者のローカルリポジトリはノンベアということです。  

#### git init テンプレート
```
git init <directory> --template=<template_directory>
```

テンプレートを使うと、事前に定義した .git サブディレクトリで新しいリポジトリを初期化できます。新しいリポジトリの .git サブディレクトリにコピーする、既定のディレクトリとファイルを設定したテンプレートを構成できます。  

### Git チュートリアル - git clone
git clone は、既存のリポジトリをターゲットとして使用する Git コマンドラインユーティリティで、ターゲットリポジトリのクローンまたはコピーを作成します。  
ユーザーの便宜のため、クローン作成を行うと、元のリポジトリを指す「origin」という名称のリモート接続が自動的に作成されます。これにより、中央リポジトリとの通信がきわめて簡単になります。この自動接続は、refs/remotes/origin にあるリモートブランチの HEAD への Git 参照を作成し、構成変数の remote.origin.url と remote.origin.fetch を初期化することで確立されます。  

**特定のフォルダーにクローンを作成する**  
```
git clone <repo> <directory>
```

**特定のタグのクローンを作成する**  
```
git clone --branch <tag> <repo>
```

#### git clone -branch
-branch 引数を使用すると、リモートの HEAD が指すブランチ (通常は main ブランチ) の代わりに、クローンを作成する特定のブランチを指定できます。また、ブランチの代わりにタグを渡しても同じ操作が可能です。  
```
git clone -branch new_feature git://remoterepository.git
```

#### git clone -mirror 対 git clone -bare
**git clone --mirror**  
--mirror 引数を渡すと、--bare 引数も暗黙的に渡されます。つまり、--mirror を指定すると、--bare 動作も継承されます。その結果、編集可能な作業ファイルを持たないベアリポジトリが作成されます。また、--mirror は、リモートリポジトリの拡張参照をすべてクローン作成し、リモートブランチの構成追跡を維持します。その後ミラーで git remote update を実行すると、元のリポジトリの参照がすべて上書きされます。完全に「ミラー」化された機能を提供します。  

**git clone --bare**  

git init --bare と同様に、-bare 引数を git clone に渡すと、省略された作業ディレクトリにリモートリポジトリのコピーが作成されます。つまり、プロジェクトの履歴を持つリポジトリが作成されます。このリポジトリでは、プッシュやプルは可能ですが、直接編集することはできません。  

### git config
git config コマンドは、グローバルまたはローカルのプロジェクト レベルで Git の構成値を設定するのに便利な機能です。これらの構成レベルは .gitconfig テキスト ファイルに対応しています。git config を実行すると、構成テキスト ファイルを編集します。ここではメールやユーザー名、エディターなどの一般的な構成設定を取り上げていきます。  

### Git エイリアス
エイリアスを使うとより長いコマンドに対応するより短いコマンドを作成できます。また、コマンドを実行する際のキー入力の手間が省けるため、より効率的なワークフローを実現できます。  

```
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.ci commit
$ git config --global alias.st status
```

#### Git エイリアスの作り方
Git 構成ファイルを直接編集する
グローバルまたはローカルの構成ファイルを手動で編集、保存してエイリアスを作成できます。グローバル構成ファイルは $HOME/.gitconfig ファイル パスにあります。ローカル構成ファイルは /.git/config のアクティブ Git リポジトリにあります。  

```
[alias]
 co = checkout
```


## HTMLのアクセシビリティについて
### で、アクセシビリティって何？
アクセシビリティというのはあなたのウェブサイトを可能な限り多くの人に利用してもらうようにすることです。かつては我々はアクセシビリティのことをハンディキャップを持つ人々のためのものだと考えていましたが、現在はモバイルデバイスや遅いネットワークを利用している人々のためのものでもあると考えられています。  

HTMLはよりセマンティック(意味的な)に使うのが大切  
(例)
```html
<div>動画を再生する</div>
```
```html
<button>動画を再生する</button>
```

HTML の `<button>` は、ある種の適切なスタイルが (おそらくそのスタイルを上書きしたいと思うでしょうが) デフォルトで適用されているだけでなく、組み込みのキーボード・アクセシビリティも備えています。つまり、`<button>` 同士の間をタブで移動できますし、リターン / エンターを使って `<button>` をアクティブにできます。  

意味的な HTML には、アクセシビリティ以外の他の利点もあります。  
- **より開発しやすい** ——上述のとおり、ある種の機能がただで手に入りますし、それに、より理解しやすいという点はまず間違いないところです。
- **モバイル機器に関して、より優れている** ——意味的な HTML は非意味的なスパゲッティ・コードよりも、ファイルサイズの点でほぼ間違いなく軽量ですし、レスポンシブにするのも簡単です。
- **SEO に関しても良好である ** ——検索エンジンは、非意味的な `<div>` などに含まれるキーワードよりも、見出しやリンクなどの中のキーワードの方に重みを持たせているので、ドキュメントが顧客に見つけてもらいやすくなるでしょう。


#### 良いセマンティクス
スクリーンリーダーのユーザーが得られる最良のアクセシビリティ支援の一つは、見出しや段落やリストなどの適切なコンテンツ構造です。きちんと意味を備えた例は、以下のようなものになるでしょう。

```html
<h1>見出し</h1>

<p>これは文書のうちの最初のセクションです。</p>

<p>ここにも、もう一つ段落を足すつもり。</p>

<ol>
  <li>ここには</li>
  <li>読んでもらいたいことの</li>
  <li>リストがあります。</li>
</ol>

<h2>下位見出し</h2>

<p>これは文書のうちの最初のサブセクションです。みんながこのコンテンツを見つけられるといいな!</p>

<h2>2 番目の下位見出し</h2>

<p>これはコンテンツのうちで 2 番目のサブセクションです。この前のものより面白いと思いますよ。</p>
```

1. コンテンツの中を進んでゆくのにつれて、スクリーンリーダーは各ヘッダを読み上げて、どれが見出しでどれが段落なのかといったことを知らせてくれます。
2. どのような速度が快適であるにせよ、その速度で進んでいけるように、スクリーンリーダーは各要素の後で停止します。
3. 多くのスクリーンリーダーでは、次の見出し / 前の見出しへとジャンプできます。
4. 多くのスクリーンリーダーでは、すべての見出しの一覧を取り出せます。それらの見出しを、特定のコンテンツを見つけるための手軽な目次のようにも使えます。


参考にするサイト [Webのアクセシビリティを向上させる、始めに取り組んでおきたいガイドラインの10項目のまとめ](https://coliss.com/articles/build-websites/operation/work/guidelines-to-improve-web-accessibility.html)  

### Webアクセシビリティとは?
W3Cによると、Webアクセシビリティとはすべての人がWebを認識し、理解し、操作し、対話し、貢献することができることを意味します。Webのアクセシビリティには、視覚、聴覚、身体、言語、認知、および神経学的障害を含む、Webへのアクセスに影響を及ぼすすべての条件が含まれます。  

#### 1. カラーに依存しない
カラーはWeb上で感情を表現し、メッセージを伝えるためによく使う強力なツールです。しかし、ユーザーに意味や情報を伝えるために、カラーに依存すべきではありません。  

![スクリーンショット 2022-02-10 19 03 06](https://user-images.githubusercontent.com/78260526/153384089-f26e8004-d260-4ef6-94ee-acc3a7cf7f5d.png)  

左は正常な人の見え方、右は色盲の人の見え方  
色盲は最も一般的な視力不足の1つです。世界の人口のおよそ4.5%に影響を与えます。

情報を伝える際の唯一の手段として、カラーを使って重要な情報を伝えると、4.5%の人が取り残されます。  

#### 2. ズームを無効にしない
レスポンシブデザインで、わたし達はいくつか間違いを犯していたかもしれません。

その中の1つは、モバイルデバイスでWebページを拡大する機能を無効にする「maximum-scale=1.0」です。  

乱視はヨーロッパとアジアの成人層30〜60％に影響を及ぼし、ぼやけた視力はすべての国籍の年齢層に影響を及ぼします  

#### 3. alt属性のあなたが知らないかもしれない事実
1. alt属性はすべてのimgタグに必須ですが、空のalt属性も有効です。画像が装飾的であるか、内容を理解する必要がない場合は、「alt=""」を使用できます。  
2. スクリーンリーダーは`<img>`タグが画像であるため、冗長にする必要はなく、altを「Picture of」で開始する必要があることをユーザーに伝えます。  
3. 画像の機能はその意味と同じくらい重要です。ロゴがWebサイトのトップページにリンクしている場合、代替テキストは「ロゴ」ではなく、リンク先を現す「トップページ」にすべきです。  
4. 代替テキストはアクセシビリティのためだけではありません。回線が遅いユーザーは、ブラウザの操作を高速化するために画像を無効にすることがあります。 alt属性を書く時は、これらのユーザが目にすることも覚えておいてください  

#### 4. 適切なマークアップを使用する
time要素はISO 8601標準を使用して日付と時間を表記する多くのフォーマット、タイムゾーン、期間が表示されます。

datetimeは`<time>`の内容を表すオプションの属性です。いくつか例を見てみましょう。  

`<a>`タグは、ファイルを別のファイルにリンクする、現在のタブまたは新しいタブにリンクを開くことを意図しています。しかし、ハンバーガーメニューや画像ギャラリーなどの操作をトリガーしたいときは理想的ではありません。button要素はこのような状況で適切な選択肢で、通常はJavaScriptで実現できます。

また、`<button>`タグは、input type="button"とよく混同されますが、違いは前者がより多くのコンテンツ（テキスト、画像+テキスト、画像のみ）を取り入れることが可能です。  

`<button>`タグを使用する際には、次の2点を考慮する必要があります。  

1. ボタンの内容が明示的ではない場合（例えば、閉じるボタンの「X」）、機能を説明するためにaria-label属性を追加する必要があります。  
```html
<button aria-label="Close">X</button>
```
2. href属性を追加すると（検索コンポーネントやライトボックスギャラリー）意味がある場合は、aタグを使用してJavaScriptでリンクの動作を上書きすることもできます。JavaScriptが有効になっていない場合、href属性を使用したこれらのギャラリーは正常に機能しなくなります。



