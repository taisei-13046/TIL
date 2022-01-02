## やったこと
先日に引き続き、React TSについてまとめていく  
今日は割とコードを書く時間が多いかも

### CORSについて
```
Access to XMLHttpRequest at  from origin 'http://localhost:3000' has been blocked by CORS policy: 
No 'Access-Control-Allow-Origin' header is present on the requested resource.
```
ReactでAPIを叩こうとしたところ、このCORSエラーが発生したため、CORSについて調べた。

**Cross-Origin Resource Sharing の略、日本語訳すると「オリジン間リソース共有」**  
#### オリジンとは  
オリジンに似ている概念に ドメイン (domain) がある  
domainとの違い  
- `ドメイン (domain)`: yahoo.co.jp
- `オリジン (origin)`: https://yahoo.co.jp:443
ドメインとの違いは、プロトコルとポート番号を含んでいる点  
`origin == protocol + domain + port number`  

#### CORSとは  
*あるオリジンで動いている Web アプリケーションに対して、別のオリジンのサーバーへのアクセスをオリジン間 HTTP リクエストによって許可できる仕組みのこと*  


#### CORSの必要性
Web セキュリティの重要なポリシーの一つに `Same-Origin Policy (同一オリジンポリシー)`がある  
これは、オリジン間のリソース共有に制限をかけるもので、次のような脆弱性を防ぐことを目的としたもの  
- XSS (Cross Site Scripting)
  - ユーザーが Web サイトにアクセスすることで不正なスクリプトが Client (Web ブラウザ) で実行されてしまう脆弱性
- CSRF (Cross-Site Request Forgeries)
  - Web アプリケーションのユーザーが、意図しない処理を Web アプリケーション (Web Server) 上で実行される脆弱性

これはサーバ再度側の設定が必要な問題  
参考資料 [なんとなく CORS がわかる...はもう終わりにする。](https://qiita.com/att55/items/2154a8aad8bf1409db2b)  
[MDN CORS](https://developer.mozilla.org/ja/docs/Web/HTTP/CORS)  

