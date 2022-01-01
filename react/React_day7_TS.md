## やったこと
ReactとTSについてまとめた  


### 関数コンポーネント
[React + TypeScript: ReactでTypeScriptを使うとき基本として知っておきたいこと](https://qiita.com/FumioNonaka/items/4361d1cdf34ffb5a5338)  


FCとVFCの違いは**childrenを暗黙的に受け付けるかどうか**  
もしもVFCを使用してChildrenをpropsで型指定する場合には、
```js
children: React.ReactNode
```  
このように指定する必要がある
しかしながら、FCとVFCはあまり使う必要はないのではないかという記事もある  
[React.FC と React.VFC はべつに使わなくていい説 ](https://kray.jp/blog/dont-have-to-use-react-fc-and-react-vfc/)  

### .ts と .tsxの違い
`.ts`
- 純粋なTypeScriptファイル
- **JSX要素の追加をサポートしない**
.tsファイルでは、JSXの記法を使用できない!!

`.tsx`
- JSXを含むファイル
- 型アサーションの記法として value as type と `<type>value` の2通りあるが、後者は .tsx には書けない  
（ `<>` はJSXタグのマーカーであるため）  

そしたら、全て.tsxにすればいいのではないかと思ってしまうが、それは良くない。  
#### .tsファイルでは明示的にJSXを使わないことを示すために使用するべき!!  

参考資料 [.ts拡張子と.tsx拡張子](https://qiita.com/y-hira18/items/f67cfc45a70c7c25708a)
