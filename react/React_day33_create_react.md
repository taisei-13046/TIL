## やったこと
環境構築!!

## 目指す環境
- Storybookを導入してchromaticで自動デプロイされる
- ESLint, prettierの設定をする
- huskyでcommit前にlint, prettierのチェックをする


### Storybookを導入してchromaticで自動デプロイされる
```
npx -p @storybook/cli sb init
```
まずはstorybookのinstall  

`.env`に`SKIP_PREFLIGHT_CHECK=true`の記述をする  
この段階でstorybookの起動はできる  

次にchromaticの導入  
```
yarn add -D chromatic
```

その後 `yarn chromatic --project-token=<project-token>`このコマンドを実行すれば、chromaticへのデプロイは成功する  

#### CI(継続的デプロイメント)の設定
pushされるたびにstorybookをデプロイされるような設定にしたい  

`.github/workflows/chromatic.yml`の設定
```
# Workflow name
name: 'Chromatic Deployment'

# Event for the workflow
on: push

# List of jobs
jobs:
  test:
    # Operating System
    runs-on: ubuntu-latest
    # Job steps
    steps:
      - uses: actions/checkout@v1
      - run: yarn
        #👇 Adds Chromatic as a step in the workflow
      - uses: chromaui/action@v1
        # Options required for Chromatic's GitHub Action
        with:
          #👇 Chromatic projectToken, see https://storybook.js.org/tutorials/intro-to-storybook/react/en/deploy/ to obtain it
          projectToken: ${{ secrets.CHROMATIC_PROJECT_TOKEN }}
          token: ${{ secrets.GITHUB_TOKEN }}
```
公式では`${{ secrets.CHROMATIC_PROJECT_TOKEN }}`にproject-tokenを上書きしろと言っているが、流石にまずいだろ  
調べたところ、githubActionsは`**github secret**`に値を設定することで環境変数として読み取れるみたい!!  

参考にした記事: 
- [chromaticでstorybookのビジュアルテスト](https://zenn.dev/kolife01/scraps/db60998387308a)  
- [secrets 公式](https://docs.github.com/ja/actions/security-guides/encrypted-secrets)

### Github Actionについて
ワークフローの構成
```
ワークフロー（YAMLファイル）
  └ jobs:
    └ ジョブ(名前は任意)
       └ steps:
         └ アクション
```

ワークフロー
- .github/workflows/ ディレクトリ以下に配置されるYAMLファイルが個別のワークフローとなる。
- 複数のワークフローを設置してもOK。
- ワークフローは並列で実行される。

ジョブ
- 各ジョブは仮想環境の新しいインスタンスで実行される。
  - したがって、ジョブ間で環境変数やファイル、セットアップ処理の結果などは共有されない。
- ジョブ間の依存を定義して待ち合わせることができる。
- データの受け渡しが必要ならアーティファクト経由で。

Step
- Jobが実行する処理の集合。
- 同じJobのStepは同じ仮想環境で実行されるのでファイルやセットアップ処理は共有できる。
- しかし各ステップは別プロセスなのでステップ内で定義した環境変数は共有できない。
  - `jobs.<job_id>.env`で定義した環境変数は全Step で利用できる

アクション
- ワークフローの最小構成要素。
- 単にrunでコマンドを実行することもできるし、Githubやサードパーティの公開アクションを利用(use)することもできる。
- コンテキストを利用して秘密情報を受け取ったりできる

[Github Actionsの使い方メモ](https://qiita.com/HeRo/items/935d5e268208d411ab5a)  

### eslint, prettierの設定

必要なパッケージのinstall
```
yarn add -D @typescript-eslint/eslint-plugin @typescript-eslint/parser eslint
```

`.eslintrc.js`ファイルを作成してESLintの設定を書いていく  
```ts
/* eslint-disable */

module.exports = {
  root: true,
  env: {
    es2021: true,
  },
  extends: [
    'eslint:recommended',
    'plugin:react/recommended',
    'plugin:@typescript-eslint/recommended',
    'plugin:react-hooks/recommended',
    'prettier',
  ],
  settings: {
    react: {
      version: 'detect',
    },
  },
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaFeatures: {
      jsx: true,
    },
    ecmaVersion: 12,
  },
  plugins: ['react', '@typescript-eslint'],
  rules: {
    '@typescript-eslint/ban-types': [
      'error',
      {
        extendDefaults: true,
        types: {
          '{}': false,
        },
      },
    ],
  },
}
```
eslintの設定は基本的にrecommendされているものを採用した  

#### 各プロパティの説明
- `root: true`  
  - ESLint は、実行時のカレントディレクトリを起点にして、上位のディレクトリの設定ファイル (.eslintrc.~) を探していく  
  - root: true の指定があると、この検索の振る舞いをそこで停止できる
  - プロジェクトのトップディレクトリに置く .eslintrc.* には、この指定をしておくとよい
- `env`
  - どのようなグローバルオブジェクトを宣言なしで参照可能にするかを ESLint に知らせるための設定
  - [ESLint - Specifying Environments](https://eslint.org/docs/user-guide/configuring/language-options#specifying-environments)  
- `extends`
  - 共有設定の適用 (Sharable configuration)
  - 複数のルールをまとめたコンフィギュレーション名を適用
  - ここで指定可能ものを sharable configuration オブジェクトと呼ぶ
  - ESLint 標準のもの（eslint:recommeded など）以外は、npm パッケージをインストールすることで指定できるようになる
  - 内部のルール設定が重複する場合は、後から指定したものが優先されることに注意
  - sharable configuration のみを提供している npm パッケージには、eslint-config- というプレフィックスが付けられており、extends プロパティで指定するときは、このプレフィックスを省略できる
    - eslint-config-airbnb パッケージ → airbnb
- `parser`
  - 使用するパーサー
  - ESLint は標準で JavaScript コードのパースに対応していますが、TypeScript コード (.ts、.tsx) を扱えるようにするには、  
    TypeScript 用のパーサー (@typescript-eslint/parser) をインストールして指定する必要がある
- `parserOption`
  - パーサーの設定
  - ESLint のデフォルトパーサーは ECMAScript 5 の構文で記述されたコードを想定している
    ```
    parserOptions:
    ecmaFeatures:
      jsx: true
    ecmaVersion: 2021   # same as 12
    sourceType: module  # use import/export
    ```
- `plugins`
  - 使用するプラグインの指定
  - ESLint プラグインを使用するには、あらかじめ npm でインストールした上で、ここに列挙しておく必要がある
- `rules`
  - 各ルールの設定
  - 個々のルール単位で有効／無効にする設定をする
  - 多くのケースでは、extends による共有設定で大まかなルール設定を行い、ここで個別ルールを細かく調整する  

rulesに今回追加した内容: {}の型指定を許可する  
[[ban-types] Should not ban {} by default!](https://github.com/typescript-eslint/typescript-eslint/issues/2063#issuecomment-675156492)  

参考にした資料 
- [Reactの開発環境を整える~husky v6で各Linterの実行まで~](https://zenn.dev/okaharuna/articles/aa715f2d9c1929)  
- [ESLint の設定ファイル (.eslintrc) の各プロパティの意味を理解する](https://maku.blog/p/j6iu7it/)

### prettierの設定
```
trailingComma: all
tabWidth: 2
semi: false
endOfLine: lf
```
今回はこのような設定を書いた  
- `trailingComma`
  - 末尾のカンマの設定
    ```
    //ES5で有効な末尾のカンマ(オブジェクト、配列など) デフォルト
    "trailingComma": "es5"

    //末尾にカンマをつけない(デフォルト)
    "trailingComma": "none"

    //可能な限り末尾にカンマを付ける(関数の引数含む) ※node 8かtransformが必要
    "trailingComma": "all"
    ```
- `semi`
  - ステートメントの最後にセミコロンを追加
    ```
    //最後にセミコロンを追加(デフォルト)
    "semi": true

    //セミコロンが無いとエラーになる箇所にだけセミコロンを追加
    "semi": false
    ```
- `endOfLine`
  - 改行の文字コードを指定
    ```
    //Linux、MacOS、gitリポジトリで一般的な、ラインフィード(\n)のみ
    "endOfLine": "lf"

    //Windowsで一般的な、キャリッジリターン + ラインフィード文字(\r\n)
    "endOfLine": "crlf"

    //キャリッジリターン文字(\r)のみ
    "endOfLine": "cr"

    //既存の行末を維持(デフォルト)
    "endOfLine": "auto"
    ```

参考記事 [.prettierrc](https://qiita.com/takeshisakuma/items/bbb2cd2f1c65de70e363#prettierrc)  




