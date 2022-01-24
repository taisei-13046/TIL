## やったこと
Reactの環境開発について学んだ  

## huskyについて
huskyとは  
**gitコマンドをhookにして、指定したコマンドを走られることができるもの**  
- npmパッケージ
- コミットやプッシュ時に、任意のコマンドを自動で実行できる
- リントやテストコマンドと組み合わせて使う

これによって手動でリントコマンドを打つ必要がなくなる  
[公式Doc](https://typicode.github.io/husky/#/)  

#### lint-stagedとの組み合わせ
lint-stagedと組み合わせることで、「git commit時にはステージング中のファイルに対してのみ」「git push時にはcommitしたファイルに対してのみ」コマンドを実行することができる  

GitのStagedされた状態とはgit addコマンドで修正されたファイルをコミットするため追加をした状態を意味する  
Staged状態のファイルは再び修正をするとgit addをしてまた追加する必要がある  
lint-stagedはStagedされたファイルを修正した後、git addをしなくても問題ないようにするツール  

#### husky & lint-staged で CI を実行する
**CIとは？**  
継続的インテグレーション（CI）とは、ソフトウェア開発においてソースコードの品質や保守性を保ち、持続可能な開発を続けていくための方法  
つまり、ほかの人が見ても理解しやすく、また自分が書いた古いコードでも処理内容が理解できるようにプログラミングしていくということ  

CI環境の導入
1. npm で husky と lint-staged、prettier、eslint をインストール
2. .huskyrc.json を用意し、pre-commit のフックに lint-staged を指定する設定を書く
3. .lintstagedrc.json を用意し、ファイルごとに実行する CI を定義


参考資料
- [qiita](https://qiita.com/shin4488/items/0a8013cc5e455f6fb25a#lint-staged%E3%81%A8%E3%81%AE%E7%B5%84%E3%81%BF%E5%90%88%E3%82%8F%E3%81%9B)
- [[React] husky, lint-staged](https://dev-yakuza.posstree.com/react/husky-lint-staged/)
- [lint-staged公式](https://github.com/okonet/lint-staged)
- [husky & lint-staged で CI を実行する](https://b-hood.site/articles/husky/#section-1)


## ESLintについて
ESLintの設定方法  
1. 設定用のファイルとしてプロジェクトのルートディレクトリーに .eslintrc.yml もしくは .eslintrc.json を作成 
  ESLint は `.eslintrc.*` もしくは `package.json` 内の `eslintConfig` 設定を読んでくれる

