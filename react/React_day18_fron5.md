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
