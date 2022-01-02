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
オリジンとは  
オリジンに似ている概念に ドメイン (domain) がある  
domainとの違い  
- `ドメイン (domain)`: yahoo.co.jp
- `オリジン (origin)`: https://yahoo.co.jp:443
ドメインとの違いは、プロトコルとポート番号を含んでいる点  
`origin == protocol + domain + port number`  

CORSとは  
*あるオリジンで動いている Web アプリケーションに対して、別のオリジンのサーバーへのアクセスをオリジン間 HTTP リクエストによって許可できる仕組みのこと*  

CORSの必要性
