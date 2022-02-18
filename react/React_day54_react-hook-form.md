## やったこと
react-hook-formの概要と実際に使ってみる

### React Hook Form
参考資料
- [React Hook Form ライブラリの使い方！フォームのバリデーションを実装しよう【 TypeScript 】](https://xn--eckhu0e2b3a6i6dsh.net/react-hook-form/)
- [React Hook Form](https://react-hook-form.com/get-started)

#### React Hook Formとは
React でフォームのバリデーションを実装するためのライブラリです。公式サイトでは、「高性能で柔軟かつ拡張可能な使いやすいフォームバリデーションライブラリ」と謳われています。  

非制御コンポーネント (Uncontrolled Components) を useForm フックに登録（ register) することで、フォームのフィールド情報を検証・収集できることをコンセプトに据えています。  

**制御コンポーネントと非制御コンポーネントの違い**  

制御・非制御コンポーネントの違いは、フォームのフィールド値を『** React 側の state として保持する**』か『**そのフィールド自身で保持する**』か  
フィールドの値を保持する管理元が異なります。  

非制御コンポーネント（ Uncontrolled Component ）とは 、フォームのフィールドそのもので値を保持します。 フィールドから値を取得するには、 useRef フックなどを使って、事前に参照を渡しておく必要があります。

```jsx
// 制御コンポーネントの具体例

import { useState } from "react";

export default function App() {
  const [state, setState] = useState<string>("");

  const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    alert("名前: " + state);
  };

  const handleChange = (e: React.FormEvent<HTMLInputElement>) => {
    setState(e.currentTarget.value);
  };

  return (
    <form onSubmit={handleSubmit}>
      <label>
        名前
        <input type="text" value={state} onChange={(e) => handleChange(e)} />
      </label>
      <button>実行する</button>
    </form>
  );
}
```

```jsx
// 非制御コンポーネントの具体例

import { useRef } from "react";

export default function App() {
  const input = useRef<HTMLInputElement>(null);

  const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    alert("名前: " + input.current);
  };

  return (
    <form onSubmit={handleSubmit}>
      <label>
        名前
        <input type="text" ref={input} />
      </label>
      <button>実行する</button>
    </form>
  );
}
```

##### sample code
```tsx
import React from "react";
import { useForm, SubmitHandler } from "react-hook-form";

interface Inputs {
  example: string;
  exampleRequired: string;
}

function App() {
  const {
    register,
    handleSubmit,
    watch,
    formState: { errors },
  } = useForm<Inputs>();
  const onSubmit: SubmitHandler<Inputs> = (data) => console.log(data);
  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input defaultValue="test" {...register("example")} />
      <input {...(register("exampleRequired"), { required: true })} />
      {errors.exampleRequired && <span>this field is required</span>}
      <input type="submit" />
    </form>
  );
}
```

**SubmitHandler** は、 TypeScript で書く場合に必要になる型定義です。 submit イベントに関連して実行する関数の型宣言に使います。  














