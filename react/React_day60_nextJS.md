## やったこと
udemyでnextJSのハンズオンをした

### Nextを学ぶモチベーション
![スクリーンショット 2022-02-24 9 43 16](https://user-images.githubusercontent.com/78260526/155435227-376c0159-e059-40c6-bb91-45fcbb8b47d4.png)  
- react-router-domがなくてもRoutingができるのすごい
- reactよりもSEO対策に有利

![スクリーンショット 2022-02-24 9 44 40](https://user-images.githubusercontent.com/78260526/155435357-18495745-e535-4767-985b-27c198061f55.png)  
- create-react-appはSEO対策が要求されるサイトには使えない

[create-react-appとnext.jsの違い](https://nextjs.org/learn/basics/data-fetching/pre-rendering)  

![スクリーンショット 2022-02-24 9 42 18](https://user-images.githubusercontent.com/78260526/155435627-36e28f86-7d8d-42b1-8e04-426a587ac1ba.png)  

![スクリーンショット 2022-02-24 9 54 01](https://user-images.githubusercontent.com/78260526/155436248-8898b918-fccb-4f81-90f0-27d4d0b2dfc6.png)


tailwind cssのチートシート
[TailwindCHEAT SHEET](https://nerdcave.com/tailwind-cheat-sheet)  

![スクリーンショット 2022-02-24 13 28 33](https://user-images.githubusercontent.com/78260526/155458090-2c8e7f03-e21f-46ca-bb85-b0202b64c9ce.png)


![スクリーンショット 2022-02-24 13 40 21](https://user-images.githubusercontent.com/78260526/155459210-cac0c79b-3353-4a82-bcca-5b5ee56273aa.png)  

## NextJSについて
### 1. Pre-rendering
SPAではページをロードする時、まず空の html を読み込んで JS ファイルも読み込んでその JS が画面をレンダリングします。このやり方では SEO でデメリットがあったりファーストビューが遅くなったりする問題があります。  

この問題を解決するため、 Next.js は基本的にすべてのページを Pre-rendering します。これはクライエント側の JS がレンダリングする代わりに、各ページに対して html を予め作っておくことを意味します。そうするとパーフォーマンスでも SEO でもより良い結果が出せます。  

![スクリーンショット 2022-02-24 14 18 01](https://user-images.githubusercontent.com/78260526/155462545-8926a3d8-6b1e-48cd-a554-c32668563930.png)  

### 2. SSG
SSG は Static Site Generation の略で、言葉通り静的サイトを生成する Pre-redndering です。この方式では html がビルド時に生成され、各リクエストに再利用されます。  

#### getStaticProps
[getStaticProps（静的生成）](https://nextjs-ja-translation-docs.vercel.app/docs/basic-features/data-fetching)  















