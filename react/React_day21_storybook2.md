## やったこと
今日は、ハッカソンのバックエンド班とミーティングしながらコードの修正をした。  
そこで以前までPOSTする際にCORSerrorと400errorが出ていた部分を解消した。  

### JSON形式で遅れていなかったが故にエラー
以前まで
```ts
const data = {
  title: recruteTitle,
  beginTime: recruteStartDate,
  endTime: recruteEndDate,
}

await http.post("/posts", data)
.then((res) => {
  console.log(res)
})
.catch((res) => console.log(res))
navigate("/home");
};
```
この形でPOSTをしていたため、400errorが出ていた。  
JSON形式で遅れていなかったためである。  
400以外の形でエラー表示してくれえよとは思ったがww  

解決策としては、
```ts
const data = JSON.stringify({
  "title": recruteTitle,
  "beginTime": recruteStartDate,
  "endTime": recruteEndDate,
})
```
`JSON.stringify()`でJSON化してからPOSTしたらちゃんと通った。  
あと、Number型で送る内容をstring型で送っていたのも問題だった。  


## Storybook * React
### 複合的なコンポーネントを組み立てる

TaskListのstoryファイル  
```js
import React from 'react';

import TaskList from './TaskList';
import * as TaskStories from './Task.stories';

export default {
  component: TaskList,
  title: 'TaskList',
  decorators: [story => <div style={{ padding: '3rem' }}>{story()}</div>],
};

const Template = args => <TaskList {...args} />;

export const Default = Template.bind({});
Default.args = {
  // Shaping the stories through args composition.
  // The data was inherited from the Default story in task.stories.js.
  tasks: [
    { ...TaskStories.Default.args.task, id: '1', title: 'Task 1' },
    { ...TaskStories.Default.args.task, id: '2', title: 'Task 2' },
    { ...TaskStories.Default.args.task, id: '3', title: 'Task 3' },
    { ...TaskStories.Default.args.task, id: '4', title: 'Task 4' },
    { ...TaskStories.Default.args.task, id: '5', title: 'Task 5' },
    { ...TaskStories.Default.args.task, id: '6', title: 'Task 6' },
  ],
};

export const WithPinnedTasks = Template.bind({});
WithPinnedTasks.args = {
  // Shaping the stories through args composition.
  // Inherited data coming from the Default story.
  tasks: [
    ...Default.args.tasks.slice(0, 5),
    { id: '6', title: 'Task 6 (pinned)', state: 'TASK_PINNED' },
  ],
};

export const Loading = Template.bind({});
Loading.args = {
  tasks: [],
  loading: true,
};

export const Empty = Template.bind({});
Empty.args = {
  // Shaping the stories through args composition.
  // Inherited data coming from the Loading story.
  ...Loading.args,
  loading: false,
};
```

#### デコレータ
```js
export default {
  component: TaskList,
  title: 'TaskList',
  decorators: [story => <div style={{ padding: '3rem' }}>{story()}</div>],
};
```
デコレーターを使ってストーリーに任意のラッパーを設定できる  
上記のコードでは、decorators というキーをデフォルトエクスポートに追加し、描画するコンポーネントの周りに padding を設定してる  

