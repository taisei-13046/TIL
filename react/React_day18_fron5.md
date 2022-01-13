## やったこと
フロントの最終課題とハッカソン準備

### default marginの消し方
レビューで以下のコードに指摘をもらった  
```css
margin: 0 0 0 16px
```
確かに、marginは省略形があるため、このコードには違和感はある。  
しかし、`margin-left`の指定だけではdefault marginは消せない  
そこで、
```css
margin-bottom: 0;
margin-left: 16px
```
とそれぞれ指定することで、記述の必要のないmargin-topとmargin-rightの無駄を省く書き方を薦められた。  
確かに納得したので、これからはこの書き方で行く  


### 400 (Bad Request)について
今日はやたらとAPIを叩くことが多い気がする  
そこで出たのが400 (Bad request)はい調べます  

#### 「400 Bad Request」エラーとは？
「400 Bad Request」（「400エラー」とも）は一般的なクライアントエラーで、エラーが他のステータスコードカテゴリのいずれにも該当しないときに返される  
ここで理解すべき重要なポイントは、400 Bad Requestエラーは、**サーバーによって処理される前に、クライアントから送信されたリクエストに関係しているということ**  

## [axios](https://github.com/axios/axios)のsource codeを読んでみる
と思ったけどむずすぎて無理やったw  
constateが限界だなw

### Features
- Make **XMLHttpRequests** from the browser
- Make http requests from **node.js**
- Supports the **Promise** API
- Intercept request and response
- Transform request and response data
- Cancel requests
- Automatic transforms for JSON data
- Client side support for protecting against XSRF

#### XMLHttpRequestとは
XMLHttpRequestはブラウザ上でサーバーとHTTP通信を行うためのAPI  
XMLHttpRequest (XHR) オブジェクトは、サーバーと対話するために使用する  
ページ全体を更新する必要なしに、データを受け取ることができる  
名前にXMLが付いていますがXMLに限ったものではなく，HTTPリクエストを投げてテキスト形式かDOMノードでレスポンスを受け取る機能を持っている  

Response Schema

```ts
{
  // `data` is the response that was provided by the server
  data: {},

  // `status` is the HTTP status code from the server response
  status: 200,

  // `statusText` is the HTTP status message from the server response
  statusText: 'OK',

  // `headers` the HTTP headers that the server responded with
  // All header names are lower cased and can be accessed using the bracket notation.
  // Example: `response.headers['content-type']`
  headers: {},

  // `config` is the config that was provided to `axios` for the request
  config: {},

  // `request` is the request that generated this response
  // It is the last ClientRequest instance in node.js (in redirects)
  // and an XMLHttpRequest instance in the browser
  request: {}
}
```

