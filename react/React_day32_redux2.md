## やったこと
先日に引き続き、Reduxのdocを読む

### Redux Thunkについて
Redux-Thunkとは、ReduxのAction Creatorに非同期処理を実装するためのミドルウェア  
通常Action CreatorはActionオブジェクトを返しますが、Redux-Thunkを使用すると「`Thunk`」という関数を返すことができるようになる  
これによってActionのディスパッチを遅らせたり、特定の条件が満たされた場合のみディスパッチできるようになる  

