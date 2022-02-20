## やったこと

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









