## やったこと
react hook formとyupについて

### useFormContext
useFormContext フックは、入れ子になっているコンポーネントに対して、必要になる props を簡単に渡せるようにするために用意されています。 React の useContext フックと同じような用途です。  

```tsx
import React from "react";
import { useForm, FormProvider, useFormContext } from "react-hook-form";

export default function App() {
  const methods = useForm();
  const onSubmit = data => console.log(data);

  return (
    <FormProvider {...methods} > // pass all methods into the context
      <form onSubmit={methods.handleSubmit(onSubmit)}>
        <NestedInput />
        <input type="submit" />
      </form>
    </FormProvider>
  );
}

function NestedInput() {
  const { register } = useFormContext(); // retrieve all hook methods
  return <input {...register("test")} />;
}
```

### useWatch
useWatch フックは、コンポーネント単位の更新をおこないます。対象のコンポーネントに useForm フックの control を渡すことで、そのコンポーネントで useWatch フックの監視機能を使えるようになります。


```tsx
import React from "react";
import { useForm, useWatch } from "react-hook-form";

function Child({ control }) {
  const firstName = useWatch({
    control,
    name: "firstName",
  });

  return <p>Watch: {firstName}</p>;
}

function App() {
  const { register, control } = useForm({
    firstName: "test"
  });
  
  return (
    <form>
      <input {...register("firstName")} />
      <Child control={control} />
    </form>
  );
}
```

### useFormState
useFormState フックは、それぞれのフォームの状態を取得できます。 useFormState フックは、他の useFormState フックや useForm フックに干渉しないので、意図しない再レンダリングを予防できます。  

```tsx
import * as React from "react";
import { useForm, useFormState } from "react-hook-form";

export default function App() {
  const { register, handleSubmit, control } = useForm({
    defaultValues: {
      firstName: "firstName"
    }
  });
  const { dirtyFields } = useFormState({
    control
  });
  const onSubmit = (data) => console.log(data);

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input {...register("firstName")} placeholder="First Name" />
      {dirtyFields.firstName && <p>Field is dirty.</p>}
      
      <input type="submit" />
    </form>
  );
}
```

### useFieldArray
useFieldArray フックは、制御されていないフィールドの配列として取り扱えます。また、配列に対して操作できる各種メソッドを取得できます。

useFieldArray フックを機能させるには、 useuseForm フックによって取得できる control オブジェクトを引数として設定する必要があります。  

```tsx
function FieldArray() {
  const { control, register } = useForm();
  const { fields, append, prepend, remove, swap, move, insert } = useFieldArray({
    control, // control props comes from useForm (optional: if you are using FormContext)
    name: "test", // unique name for your Field Array
  });

  return (
    {fields.map((field, index) => (
      <input
        key={field.id} // important to include key with field's id
        {...register(`test.${index}.value`)} 
      />
    ))}
  );
}
```


## Yup
[yup github](https://github.com/jquense/yup)  

> Killer Features:

> - Concise yet expressive schema interface, equipped to model simple to complex data models
> - Powerful TypeScript support. Infer static types from schema, or ensure schema correctly implement a type
> - Built-in async validation support. Model server-side and client-side validation equally well
> - Extensible: add your own type-safe methods and schema
> - Rich error details, make debugging a breeze

↓ 翻訳

- シンプルなデータモデルから複雑なデータモデルまで対応可能な、簡潔かつ表現力豊かなスキーマインターフェイス。
- TypeScriptを強力にサポート。スキーマから静的な型を推論したり、スキーマが正しく型を実装していることを確認することができます。
- 非同期検証をサポート。サーバーサイドとクライアントサイドの検証を同じようにモデル化します。
- 拡張性：独自のタイプセーフなメソッドやスキーマを追加可能
- 豊富なエラーの詳細、デバッグを容易にします。


### Getting Started
```tsx
import { object, string, number, date, InferType } from 'yup';

let userSchema = object({
  name: string().required(),
  age: number().required().positive().integer(),
  email: string().email(),
  website: string().url().nullable(),
  createdOn: date().default(() => new Date()),
});

// parse and assert validity
const user = await userSchema.validate(await fetchUser());

type User = InferType<typeof userSchema>;
/* {
  name: string;
  age: number;
  email?: string | undefined
  website?: string | null | undefined
  createdOn: Date
}*/
```

### Yup と React Hook Formの組み合わせ

```tsx
import React from "react";
import { useForm } from "react-hook-form";
import { yupResolver } from '@hookform/resolvers/yup';
import * as yup from "yup";

interface IFormInputs {
  firstName: string
  age: number
}

const schema = yup.object().shape({
  firstName: yup.string().required(),
  age: yup.number().positive().integer().required(),
}).required();

export default function App() {
  const { register, handleSubmit, errors } = useForm<IFormInputs>({
    resolver: yupResolver(schema)
  });
  const onSubmit = (data: IFormInputs) => console.log(data);

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input name="firstName" ref={register} />
      <p>{errors.firstName?.message}</p>
        
      <input name="age" ref={register} />
      <p>{errors.age?.message}</p>
      
      <input type="submit" />
    </form>
  );
}
```






