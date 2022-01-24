## やったこと
Reactの環境開発について学んだ  

### huskyについて
huskyとは  
**gitコマンドをhookにして、指定したコマンドを走られることができるもの**  
- npmパッケージ
- コミットやプッシュ時に、任意のコマンドを自動で実行できる
- リントやテストコマンドと組み合わせて使う

これによって手動でリントコマンドを打つ必要がなくなる  
[公式Doc](https://typicode.github.io/husky/#/)  

#### lint-stagedとの組み合わせ
lint-stagedと組み合わせることで、「git commit時にはステージング中のファイルに対してのみ」「git push時にはcommitしたファイルに対してのみ」コマンドを実行することができる  

参考資料
- [qiita](https://qiita.com/shin4488/items/0a8013cc5e455f6fb25a#lint-staged%E3%81%A8%E3%81%AE%E7%B5%84%E3%81%BF%E5%90%88%E3%82%8F%E3%81%9B)
- [[React] husky, lint-staged](https://dev-yakuza.posstree.com/react/husky-lint-staged/)

