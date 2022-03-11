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








