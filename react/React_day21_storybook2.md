## やったこと
今日は、ハッカソンのバックエンド班とミーティングしながらコードの修正をした。  
そこで以前までPOSTする際にCORSerrorと400errorが出ていた部分を解消した。  

### JSON形式で遅れていなかったが故にエラー
以前まで
```ts
const data = {
  title: recruteTitle,
  beginTime: recruteStartDate,
  endTime: recruteEndDate,
}

await http.post("/posts", data)
.then((res) => {
  console.log(res)
})
.catch((res) => console.log(res))
navigate("/home");
};
```
この形でPOSTをしていたため、400errorが出ていた。  
JSON形式で遅れていなかったためである。  
400以外の形でエラー表示してくれえよとは思ったがww  

解決策としては、
```ts
const data = JSON.stringify({
  "title": recruteTitle,
  "beginTime": recruteStartDate,
  "endTime": recruteEndDate,
})
```
`JSON.stringify()`でJSON化してからPOSTしたらちゃんと通った。  
あと、Number型で送る内容をstring型で送っていたのも問題だった。  


