## やったこと
ReachartsでPieChartを書く際のポイント

### useCallbackのdepsに迷った時のESLint
[[ESLint] Feedback for 'exhaustive-deps' lint rule](https://github.com/facebook/react/issues/14920)  

 ESLintがdepsの最適解を教えてくれる！便利！
 
### ReCharts
 [PieChart](https://recharts.org/en-US/api/PieChart)  
 
#### ResponsiveContainer
 [ResponsiveContainer](https://recharts.org/en-US/api/ResponsiveContainer)を使うことで簡単にレスポンシブ対応することができる

`aspect, width, height, minWidth, minHeight, debounce`を引数に取る (optional)  

#### PieChart
```jsx
<PieChart width={730} height={250}>
  <Pie data={data01} dataKey="value" nameKey="name" cx="50%" cy="50%" outerRadius={50} fill="#8884d8" />
  <Pie data={data02} dataKey="value" nameKey="name" cx="50%" cy="50%" innerRadius={60} outerRadius={80} fill="#82ca9d" label />
</PieChart>
```

**Parent Components**  
`<ResponsiveContainer />`  

**Child Components**  
`<PolarAngleAxis /> <PolarRadiusAxis /> <PolarGrid /> <Legend /> <Tooltip /> <Pie /> <Customized />`  

#### Pie
**Child Components**  
`<Cell /> <LabelList />`  

```ts
        <Pie
          width={width}
          height={height}
          data={data}
          dataKey="value"
          label={label}
          labelLine={false}
          startAngle={90}
          endAngle={-270}
          innerRadius={50}
          outerRadius={100}
          blendStroke
        >
```

- width, heightはPieの大きさを指定している  
- dataにはオブジェクトで`{value: string, label: string}`を渡す
- dataKeyにvalueを渡すとdataのkeyになる
- startAngleはPieChartの開始地点
 - 初期は180から始まるため、右横から回る設定
- innerRadiusは内側の余白
- outerRadiusは外側の余白

```tsx
<PieChart width={730} height={250}>
  <Pie data={data} cx="50%" cy="50%" outerRadius={80} label>
    {
      data.map((entry, index) => (
        <Cell key={`cell-${index}`} fill={colors[index]}/>
      ))
    }
  </Pie>
</PieChart>
```

#### `<Cell />`
`<Cell />`には書くdataをmapで展開して表示していく  

### labelを生成するために
labelを書くCellの中心の延長線上に配置したい。  
そのx座標, y座標を求めるために。。。  

```ts
const x =
  cx + (outerRadius + LABEL_RADIUS_PADDING) * Math.cos(-radian(midAngle))
const y =
  cy + (outerRadius + LABEL_RADIUS_PADDING) * Math.sin(-radian(midAngle))
```

cx, cyとは

> The x-coordinate of center. If set a percentage, the final value is obtained by multiplying the percentage of container width.  
> The y-coordinate of center. If set a percentage, the final value is obtained by multiplying the percentage of container height.  
> DEFAULT: '50%'  



#### ラジアンの計算
[度をラジアンに変換する方法](https://lab.syncer.jp/Web/JavaScript/Snippet/51/#:~:text=JavaScript%E3%81%A7%E8%A7%92%E5%BA%A6%E3%82%92%E5%8F%96%E3%82%8A%E6%89%B1%E3%81%86,180%20%E3%82%92%E3%81%8B%E3%81%91%E3%81%BE%E3%81%97%E3%82%87%E3%81%86%E3%80%82)

JavaScriptで角度を取り扱う時は、単位をラジアンにするのが一般的です。度をラジアンに変換するには、値にMath.PI/180をかけましょう。

```js
var radian = degree * ( Math.PI / 180 ) ;

```

```ts
export function radian(degree: number): number {
  return (Math.PI / 180) * degree
}
```







