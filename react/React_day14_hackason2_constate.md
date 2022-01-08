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

### constateについて
contextを分離させるために、頑張った結果が以下の写真だ  
<img width="882" alt="スクリーンショット 2022-01-08 13 39 51" src="https://user-images.githubusercontent.com/78260526/148631642-92fa73af-513c-4ab3-ab81-ac616310072e.png">  
実装としては正解らしい。。。が、みてられないくらい量が多い　　
これを解決してくれるのが`constate`である。  
[github source code](https://github.com/diegohaz/constate)  

使用方法
```shell
$ npm install constate --save
# or
$ yarn add constate
```
まずはinstallから  

設置
```js
import { useState } from 'react';
import constate from 'constate';

const [CountProvider, useCountContext] = constate(() => {
  const [count] = useState(0);
  return count;
});
```

constateを使った結果

```js
export const [RecruteTitleProvider, useRecruteTitleContext] = constate(() => {
  const [recruteTitle, setRecruteTitle] =
    useState<string | undefined>(undefined);
  return { recruteTitle, setRecruteTitle };
});

export const [RecruteUseLanguageProvider, useRecruteUseLanguageContext] =
  constate(() => {
    const [recruteUseLanguage, setRecruteUseLanguage] =
      useState<string | undefined>(undefined);
    return { recruteUseLanguage, setRecruteUseLanguage };
  });

export const [RecruteDateProvider, useRecruteDateContext] = constate(() => {
	const [recruteStartDate, setRecruteStartDate] = useState<Date | null | undefined>(null)
	const [recruteEndDate, setRecruteEndDate] = useState<Date | null | undefined>(null)
	return { recruteStartDate, setRecruteStartDate, recruteEndDate, setRecruteEndDate }
})

export const RecrutePostProvider = (props: { children: ReactNode }) => {
  const { children } = props;

  return (
    <RecruteTitleProvider>
      <RecruteUseLanguageProvider>
				<RecruteDateProvider>
					{children}
				</RecruteDateProvider>
			</RecruteUseLanguageProvider>
    </RecruteTitleProvider>
  );
};
```
だいぶんスッキリした気がする  

参考資料
- [React Context + State](https://bestofreactjs.com/repo/diegohaz-constate-react-awesome-react-hooks)

