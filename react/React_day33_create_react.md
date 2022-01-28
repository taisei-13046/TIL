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
