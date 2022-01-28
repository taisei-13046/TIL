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

