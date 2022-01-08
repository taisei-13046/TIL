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
export const [RecruteTitleProvider, useRecruteTitleContext, useSetRecruteTitleContext] = constate(() => {
  const [recruteTitle, setRecruteTitle] =
    useState<string | undefined>(undefined);
  return { recruteTitle, setRecruteTitle };
},
  value => value.recruteTitle,
  value => value.setRecruteTitle
);

export const [RecruteUseLanguageProvider, useRecruteUseLanguageContext, useSetRecruteUseLanguageContext] =
  constate(() => {
    const [recruteUseLanguage, setRecruteUseLanguage] =
      useState<string | undefined>(undefined);
    return { recruteUseLanguage, setRecruteUseLanguage };
  },
    value => value.recruteUseLanguage,
    value => value.setRecruteUseLanguage
  );

export const [RecruteDateProvider, useRecruteStartDateContext, useSetStartRecruteDateContext, useEndRecruteDateContext, useSetEndRecruteDateContext] = constate(() => {
	const [recruteStartDate, setRecruteStartDate] = useState<Date | null | undefined>(null)
	const [recruteEndDate, setRecruteEndDate] = useState<Date | null | undefined>(null)
	return { recruteStartDate, setRecruteStartDate, recruteEndDate, setRecruteEndDate }
},
  value => value.recruteStartDate,
  value => value.setRecruteStartDate,
  value => value.recruteEndDate,
  value => value.setRecruteEndDate,
)

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
こうなる  
しかし、**実際にContextを使うと依存関係が増えるためなるべく使わない設計にするべき**  

### 簡潔なまとめ
上のだとちょっとみずらいので簡潔なパターンを掲示  

```js
export const [countProvider, useCountContext, useSetCountContext] = constate(() => {
  const [count, setCount] =
    useState<number>(0);
  return { count, setCount };
  },
  value => value.count,
  value => value.setCount
);
```
- constateの第二引数以降に、第一引数の関数の返り値をセレクターで分割することで、内部でContext分割してくれる
- この場合だとcountProviderは2つのProviderを内包するReactNodeになる
- 残りの二つのHookはそれぞれのConsumerになる

#### ReactNodeとは
[Reactのコンポーネント周りの用語を整理する](https://blog.ojisan.io/react-component-words/)  
```ts
type ReactNode =
  | ReactChild
  | ReactFragment
  | ReactPortal
  | boolean
  | null
  | undefined
```
という定義になっている  
ReactNodeが使われるのは、createElementの引数  
```ts
function createElement<P extends HTMLAttributes<T>, T extends HTMLElement>(
  type: keyof ReactHTML,
  props?: (ClassAttributes<T> & P) | null,
  ...children: ReactNode[]
): DetailedReactHTMLElement<P, T>
```



参考資料
- [React Context + State](https://bestofreactjs.com/repo/diegohaz-constate-react-awesome-react-hooks)
- [ソースコード](https://github.com/diegohaz/constate/blob/master/src/index.tsx)


