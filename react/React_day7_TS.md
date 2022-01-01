## やったこと
ReactとTSについてまとめた  

[React + TypeScript: ReactでTypeScriptを使うとき基本として知っておきたいこと](https://qiita.com/FumioNonaka/items/4361d1cdf34ffb5a5338)  

### 関数コンポーネント

FCとVFCの違いは**childrenを暗黙的に受け付けるかどうか**  
もしもVFCを使用してChildrenをpropsで型指定する場合には、
```js
children: React.ReactNode
```  
このように指定する必要がある
しかしながら、FCとVFCはあまり使う必要はないのではないかという記事もある  
[React.FC と React.VFC はべつに使わなくていい説 ](https://kray.jp/blog/dont-have-to-use-react-fc-and-react-vfc/)  

