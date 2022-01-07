## やったこと
ハッカソンの準備

### useContextでのエラー
```js
  <ApplyUseLanguageContext.Provider value={applyUseLanguage} >
  </ApplyUseLanguageContext.Provider>
```
上のように単体でContextに値を入れた場合、ずっとオブジェクトとして渡されると思っていた  

そのため、下のように
```js
	const { applyStartDate } = useContext(ApplyStartDateContext)
```
オブジェクトとして受け取っていたらずっと `~~ does not exists type ~~`のエラーが出ていた  
正しい回答としては、
```js
	const applyStartDate = useContext(ApplyStartDateContext)
```
である。
