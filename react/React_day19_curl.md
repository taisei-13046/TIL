## やったこと
curlコマンドについて調べた  

### curlコマンドでPOSTリクエスト

**POSTリクエストを送るには、`-X POST` を付与する** 

```shell
$ curl -w '\n' 'http://localhost:8080/createItem' --data 'name=sample' -X POST
```
`--data` またはその省略系である `-d` で、POSTで送信するデータを記述  
しかし、`--data`にfileを与えたいときは、あらかじめエンコードする必要がある  
その場合は、`--data-urlencode`を使うと、curlがエンコードしてくれるので便利  

#### CRUD操作をする場合のそれぞれのカンペ
GET
```shell
$ curl http://localhost:3000/api/v1/articles/4
```
getリクエストの場合は`Xオプション`は必要ない  

POST
```shell
$ curl http://localhost:3000/api/v1/articles -XPOST -d 'title=posted title&body=posted body'
```
-dオプションの後にデータを記述できる

PUT
```shell
$ curl http://localhost:3000/api/v1/articles/7 -XPUT -d 'title=updated title&body=updated body'
```
`-XPUT`を指定するほかはPOSTと同じ

DELETE
```shell
$ curl http://localhost:3000/api/v1/articles/7 -XDELETE
```
`-XDELETE`を指定する

参考資料
- [curl コマンド 使い方メモ](https://qiita.com/yasuhiroki/items/a569d3371a66e365316f)
- [【curl】超入門(GET/POST/PUT/DELETEでリクエスト)[LINUX]](https://qiita.com/takuyanin/items/949201e3eb100d4384e1#http%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89)


## PG Dashboadのコードを頑張って読む
まず、コードの量が多すぎて何が何だかさっぱりだったのが正直なところ。  
その中でも、自分の知識になりそうなところをピックアップしていきたい  

### axiosのインスタンスを作成していた
最近、axiosをそのまま使うのではなくて、instanceをcreateしてから使うようにしていたが、その方法が取られていたので、  
自分のやり方は間違ってはいなかったのだと感じた  

```ts
const instance = axios.create({
  baseURL: config.api_url,
})
```

### axiosの型を定義していた
コードをみると、
```ts
import axios, { AxiosInstance, AxiosRequestConfig } from 'axios'
```
知らない型をimportしていた。  
よくわからないので調べてみる  

参考にした資料 [【TypeScript】Axiosのリクエストとレスポンスに型をつける](https://zenn.dev/mkt_engr/articles/axios-req-res-typescript)  

#### AxiosRequestConfigについて
この型は、

```ts
const config: AxiosRequestConfig = {
  url: `${url}/users`,
  method: "GET",
};
axios(config);
```
このようにoptionsに割り当てる方である。  
型の定義を見てみると,  
```ts
export interface AxiosRequestConfig<D = any> {
  url?: string;
  method?: Method;
  baseURL?: string;
  transformRequest?: AxiosRequestTransformer | AxiosRequestTransformer[];
  transformResponse?: AxiosResponseTransformer | AxiosResponseTransformer[];
  headers?: AxiosRequestHeaders;
  params?: any;
  paramsSerializer?: (params: any) => string;
  data?: D;
  timeout?: number;
  timeoutErrorMessage?: string;
  withCredentials?: boolean;
  adapter?: AxiosAdapter;
  auth?: AxiosBasicCredentials;
  responseType?: ResponseType;
  responseEncoding?: responseEncoding | string;
  xsrfCookieName?: string;
  xsrfHeaderName?: string;
  onUploadProgress?: (progressEvent: any) => void;
  onDownloadProgress?: (progressEvent: any) => void;
  maxContentLength?: number;
  validateStatus?: ((status: number) => boolean) | null;
  maxBodyLength?: number;
  maxRedirects?: number;
  socketPath?: string | null;
  httpAgent?: any;
  httpsAgent?: any;
  proxy?: AxiosProxyConfig | false;
  cancelToken?: CancelToken;
  decompress?: boolean;
  transitional?: TransitionalOptions;
  signal?: AbortSignal;
  insecureHTTPParser?: boolean;
}
```
このようになっている  
これに対しての詳しい説明がdocumentに書かれている  
[axios Request Config](https://axios-http.com/docs/req_config)  

