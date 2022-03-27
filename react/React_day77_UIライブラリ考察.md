## やったこと
UIライブラリを考察して適切な命名を考える

MUIを参照

### color
色の指定の際に色名(grayなど)を直接指定するのではなく、抽象的な指定の仕方をしている

```
'inherit'
| 'primary'
| 'secondary'
| 'success'
| 'error'
| 'info'
| 'warning'
| string
```

Palette colors
The theme exposes the following palette colors (accessible under theme.palette.):

- **primary** - used to represent primary interface elements for a user. It's the color displayed most frequently across your app's screens and components.
- **secondary** - used to represent secondary interface elements for a user. It provides more ways to accent and distinguish your product. Having it is optional.
- **error** - used to represent interface elements that the user should be made aware of.
- **warning** - used to represent potentially dangerous actions or important messages.
- **info** - used to present information to the user that is neutral and not necessarily important.
- **success** - used to indicate the successful completion of an action that user triggered.

[button API](https://mui.com/api/button/)  


### variant
variant自体は変化形という抽象的な意味合いを持つ。  

MUIのbutton要素のvariantは以下のように使われている

```
'contained'
| 'outlined'
| 'text'
| string
```

**色はcolorで(なるべ抽象的に)形はvariantでまとめていくのが良さそう**  

### Boolean系
今まで自分は`is~~~`とか、`can~~~`とかで全てbooleanであることを明確にしながら命名をしてきた。  
しかし、意外にそう言った命名は少なく、

```
⭕️
expanded
square

❎
isExpanded
isSquare
```

expandedで「何かが開いてるかどうか」を意味する。  
確かに、`is~~~`といった形にしなくても伝わるので、冗長な命名はやめようかなと思う  

### onハンドラ属性

例えばAlertコンポーネントを例にとると、
アラートを閉じる時のハンドラの命名は`onClose`と命名されている。  







