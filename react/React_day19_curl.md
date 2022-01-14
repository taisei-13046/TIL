## やったこと
curlコマンドについて調べた  

### curlコマンドでPOSTリクエスト

**POSTリクエストを送るには、`-X POST` を付与する ** 

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
