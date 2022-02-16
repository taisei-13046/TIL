## やったこと
Reactの公式を読んだ!4

### フックの導入
#### 動機
**ステートフルなロジックをコンポーネント間で再利用するのは難しい**  
React は再利用可能な振る舞いをコンポーネントに「付加する」方法（例えばストアオブジェクトを connect するなど）を提供していません。React をしばらく使った事があれば、この問題を解決するためのレンダープロップや高階コンポーネントといったパターンをご存じかもしれません。しかしこれらのパターンを使おうとするとコンポーネントの再構成が必要であり、面倒なうえにコードを追うのが難しくなります。典型的な React アプリを React DevTools で見てみると、おそらくプロバイダやらコンシューマやら高階コンポーネントやらレンダープロップやら、その他諸々の抽象化が多層に積み重なった『ラッパー地獄』を見ることになるでしょう。DevTools でそれらをフィルタして隠すことはできますが、この背景にはもっと根本的な問題があるということがわかります  

フックを使えば、ステートを持ったロジックをコンポーネントから抽出して、単独でテストしたり、また再利用したりすることができます。フックを使えば、ステートを持ったロジックを、コンポーネントの階層構造を変えることなしに再利用できるのです。  

**複雑なコンポーネントは理解しづらくなる**  
我々はよく、最初はシンプルだったのに、state を使うロジックや副作用によって管理不能なごちゃ混ぜ状態に陥ってしまったコンポーネントをメンテナンスさせられてきました。それぞれのライフサイクルメソッドには、しばしば互いに関係のないロジックが混在してしまいます。例えばとあるコンポーネントは componentDidMount と componentDidUpdate で何かデータを取得しているかもしれません。しかし同じ componentDidMount 内には、イベントリスナを登録する何か無関係なロジックがあるかもしれませんし、そのクリーンアップのコードは componentWillUnmount に書かれているかもしれません、といった具合です。一緒に更新されるべき互いに関連したコードがバラバラにされ、一方でまったく無関係なコードが 1 つのメソッド内に書かれています。このような状態は簡単にバグや非整合性を引き起こします。  

この問題を解決するため、関連する機能（例えばデータの購読や取得）をライフサイクルメソッドによって無理矢理分割する代わりに、フックは関連する機能に基づいて、1 つのコンポーネントを複数の小さな関数に分割することを可能にします。  

**クラスは人間と機械の両方を混乱させる**  
コードの再利用や整頓が難しくなるということに加えてクラスについて我々が学んだことは、クラスが React を学ぶ上で障壁となっているということです。JavaScript で this がどのように動作するのか理解しなければなりませんが、それは他の多くの言語での動作とは非常に異なっています。イベントハンドラを bind するよう覚えておく必要があります。仕様が不確定な提案中の構文を使わない限り、コードは非常に冗長になってしまいます。開発者は props や state やトップダウンのデータフローについて完璧に理解できても、クラスの部分でつまづいてしまいます。React における関数コンポーネントとクラスコンポーネントの違いや使い分けについては経験のある React 開発者の間でも意見の差異が出てきます。  

これらの問題を解決するため、フックは、より多くの React の機能をクラスを使わずに利用できるようにします。コンセプト的には、React のコンポーネントは常に関数に近いものでした。フックは関数を活用しながらも、React の実用性を犠牲にしません。フックは命令型コードへの避難ハッチへのアクセスを提供しますし、複雑な関数型プログラミングやリアクティブプログラミングの技法を学ばせることもありません。  

### フック早わかり

フックとは、関数コンポーネントに state やライフサイクルといった React の機能を “接続する (hook into)” ための関数です。フックは React をクラスなしに使うための機能ですので、クラス内では機能しません。  

#### 副作用フック
useEffect は副作用のためのフックであり、関数コンポーネント内で副作用を実行することを可能にします。クラスコンポーネントにおける componentDidMount, componentDidUpdate および componentWillUnmount と同様の目的で使うものですが、1 つの API に統合されています  

```jsx
const Component = () => {
  useEffect(() => {
    // componentDidMount のタイミングで実行したい処理を記述
    return () => {
      // componentWillUnmount のタイミングで実行したい処理を記述
    }
  }, []);
};
```

#### componentWillUnmount()のタイミングがいまいちわからない

[Reactの基礎 (Lifecycle)](https://programmagick.com/sections/react/lifecycle#commmponent_willunmount)  
[componentWillUnmount](https://js.studio-kingdom.com/react/component_lifecycle/unmounting_componentwillunmount)  

componentWIllUnmount() はコンポーネントがアンマウントされ、破棄される直前に呼び出されます。このメソッド中では、setInterval()を使ったタイマーのような処理を解除したり、何らかのデータの購読を解除したりする処理を記述します。  

DOMからコンポーネントがアンマウントされる直前に実行されます。
タイマーの無効化やcomponentDidMountで作成されたDOMの後片付けのような、 何らかのクリーンアップが必要な際には、このメソッドの中でそれを実行して下さい。  

#### マウントって何？
[ReactのMountとは何か](https://gist.github.com/kenmori/7996ff836bf4ec5f08088eff55c1442d)  
[What is "Mounting" in React js?](https://stackoverflow.com/questions/31556450/what-is-mounting-in-react-js)  
Reactコンポーネントに対応するインスタンスとDOMノードを作成し、それらをDOMに挿入するこのプロセスがマウントと呼ばれます。  

#### フックのルール
- フックは関数のトップレベルのみで呼び出してください。ループや条件分岐やネストした関数の中でフックを呼び出さないでください。
- フックは React の関数コンポーネントの内部のみで呼び出してください。通常の JavaScript 関数内では呼び出さないでください（ただしフックを呼び出していい場所がもう 1 カ所だけあります — 自分のカスタムフックの中です。これについてはすぐ後で学びます）。

### 副作用フックの利用法
React のライフサイクルに馴染みがある場合は、useEffect フックを componentDidMount と componentDidUpdate と componentWillUnmount がまとまったものだと考えることができます。  

React コンポーネントにおける副作用には 2 種類あります。クリーンアップコードを必要としない副作用と、必要とする副作用です。  

#### クリーンアップを必要としない副作用
時に、React が DOM を更新した後で追加のコードを実行したいという場合があります。ネットワークリクエストの送信、手動での DOM 改変、ログの記録、といったものがクリーンアップを必要としない副作用の例です。  

React のクラスコンポーネントでは、render メソッド自体が副作用を起こすべきではありません。そこで副作用を起こすのは早すぎます — 典型的には、副作用は React が DOM を更新したあとに起こすようにしたいのです。  

**useEffect は何をやっているのか？**   

このフックを使うことで、レンダー後に何かの処理をしないといけない、ということを React に伝えます。React はあなたが渡した関数を覚えており（これを「副作用（関数）」と呼ぶこととします）、DOM の更新の後にそれを呼び出します。この副作用の場合はドキュメントのタイトルをセットしていますが、データを取得したりその他何らかの命令型の API を呼び出したりすることも可能です。

**useEffect がコンポーネント内で呼ばれるのはなぜか?**  

コンポーネント内で useEffect を記述することで、副作用内から state である count（や任意の props）にアクセスできるようになります。それらは既に関数スコープ内に存在するので、参照するための特別な API は必要ありません。フックは JavaScript のクロージャを活用しており、JavaScript で解決できることに対して React 特有の API を導入することはしません。

**useEffect は毎回のレンダー後に呼ばれるのか？**  

その通りです！ デフォルトでは、副作用関数は初回のレンダー時および毎回の更新時に呼び出されます。「マウント」と「更新」という観点で考えるのではなく、「レンダーの後」に副作用は起こる、というように考える方が簡単かもしれません。React は、副作用が実行される時点では DOM が正しく更新され終わっていることを保証します。


componentDidMount や componentDidUpdate と異なり、useEffect でスケジュールされた副作用はブラウザによる画面更新をブロックしません。このためアプリの反応がより良く感じられます。大部分の副作用は同期的に行われる必要がありません。同期的に行う必要がある稀なケース（レイアウトの測定など）のために、useEffect と同一の API を有する useLayoutEffect という別のフックがあります。  

#### クリーンアップを有する副作用
**副作用内からなぜ関数を返したのか？**  

これこそが副作用のクリーンアップのためのオプションの仕組みです。すべての副作用は、それをクリーンアップするための関数を返すことができます。これにより購読を開始するためのロジックと解除するためのロジックを並べて書くことができます。両方とも同じ副作用の一部なのです！

**React は具体的には副作用のクリーンアップをいつ発生させるのか？**  

React はコンポーネントがアンマウントされるときにクリーンアップを実行します。しかし、すでに学んだ通り、副作用は 1 回だけでなく毎回のレンダー時に実行されます。このため React は、ひとつ前のレンダーによる副作用を、次回の副作用を実行する前にもクリーンアップします。この後で、これがなぜバグの回避につながるのか、そしてこれがパフォーマンスの問題を引き起こしている場合にどのようにしてこの挙動を止めるのかについて説明します。


#### 副作用を使う場合のヒント
**関心を分離するために複数の副作用を使う**  

```jsx
function FriendStatusWithCounter(props) {
  const [count, setCount] = useState(0);
  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });

  const [isOnline, setIsOnline] = useState(null);
  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }

    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });
  // ...
}
```

フックを使うことで、ライフサイクルのメソッド名に基づくのではなく、**実際に何をやっているのかに基づいて**コードを分割できます。  
React はコンポーネントで利用されているすべての副作用を、**指定されている順番**で適用していきます。

#### 副作用のスキップによるパフォーマンス改善  

```jsx
useEffect(() => {
  document.title = `You clicked ${count} times`;
}, [count]); // Only re-run the effect if count changes
```

この最適化を利用する場合、時間の経過とともに変化し副作用によって利用される、コンポーネントスコープの値（props や state など）がすべて配列に含まれていることを確認してください。さもないとあなたのコードは以前のレンダー時の古い値を参照してしまうことになります。  

### フックのルール
#### フックを呼び出すのはトップレベルのみ
フックをループや条件分岐、あるいはネストされた関数内で呼び出してはいけません。代わりに、あなたの React の関数のトップレベルでのみ、あらゆる早期 return 文よりも前の場所で呼び出してください。これを守ることで、コンポーネントがレンダーされる際に毎回同じ順番で呼び出されるということが保証されます。これが、複数回 useState や useEffect が呼び出された場合でも React がフックの状態を正しく保持するための仕組みです  

React は、どの useState の呼び出しがどの state に対応するのか、どうやって知るのでしょうか？ その答えは「**React はフックが呼ばれる順番に依存している**」です。  

```jsx
// ------------
// First render
// ------------
useState('Mary')           // 1. Initialize the name state variable with 'Mary'
useEffect(persistForm)     // 2. Add an effect for persisting the form
useState('Poppins')        // 3. Initialize the surname state variable with 'Poppins'
useEffect(updateTitle)     // 4. Add an effect for updating the title

// -------------
// Second render
// -------------
useState('Mary')           // 1. Read the name state variable (argument is ignored)
useEffect(persistForm)     // 2. Replace the effect for persisting the form
useState('Poppins')        // 3. Read the surname state variable (argument is ignored)
useEffect(updateTitle)     // 4. Replace the effect for updating the title

// ...
```

フックへの呼び出しの順番がレンダー間で変わらない限り、React はそれらのフックにローカル state を割り当てることができます。ですがフックの呼び出しを条件分岐内（例えば persistForm 副作用の内部で）で行ったらどうなるでしょうか？  

```jsx
  // 🔴 We're breaking the first rule by using a Hook in a condition
  if (name !== '') {
    useEffect(function persistForm() {
      localStorage.setItem('formData', name);
    });
  }
```

name !== '' という条件は初回のレンダー時には true なので、フックは実行されます。しかし次回のレンダー時にはユーザがフォームをクリアしているかもしれず、その場合にこの条件は false になります。するとレンダー途中でこのフックがスキップされるため、フックの呼ばれる順番が変わってしまいます。

React は 2 つ目の useState の呼び出しに対して何を返せばいいのか分からなくなります。React は 2 つめのフックの呼び出しは前回レンダー時と同様に persistForm に対応するものだと期待しているのですが、それが成り立たなくなっています。この部分より先では、スキップされたもの以降のすべてのフックがひとつずつずれているため、バグを引き起こします。

これがフックを呼び出すのがトップレベルのみでなければならない理由です。  



