## やったこと
udemyの復習  

### customHook
- ただの関数
- コンポーネントからロジックを分離
- 使い回し、テスト容易、見通しが良くなる
- `use ~` から始める

#### まずはcustomHookを使用しないでaxiosでAPIを取得する
<details>
  <summary>customHook使用前</summary>
  
```ts
export default function App() {
  const [userProfile, setUserProfile] = useState<UserProfile[]>([])
  const [loading, setLoading] = useState(false)
  const [error, setError] = useState(false)

  const onClickFetchUser = () => {
    setLoading(true)
    setError(false)

    axios.get<User[]>("https://jsonplaceholder.typicode.com/users")
    .then((res) => {
      const data = res.data.map((user) => ({
        id: user.id,
        name: `${user.name} (${user.username})`,
        email: user.email,
        address: `${user.address.city}${user.address.suite}${user.address.street}`
      }))
      setUserProfile(data)
    })
    .catch(() => {
      setError(true)
    })
    .finally(() => {
      setLoading(false)
    })
  };

  return (
    <div className="App">
      <button onClick={onClickFetchUser}>データ取得</button>
      <br />
      {error ? (
        <>
          <p style={{color: "red"}}>データの取得に失敗しました</p>
        </>
      ): (
        loading? (
          <p>Loading....</p>
        ): (
        <>
        {userProfile.map((user) => (
          <UserCard user={user} key={user.id} />
        ))}
        <UserCard user={user} />
        </>)
      )}
      
    </div>
  );
}
```
</details>  
  
#### ここでのポイント
**API取得までの間はLoadingを表示する**

```ts
const onClickFetchUser = () => {
    setLoading(true)
    setError(false)

    axios.get<User[]>("https://jsonplaceholder.typicode.com/users")
    .then((res) => {
    })
    .catch(() => {
      setError(true)
    })
    .finally(() => {
      setLoading(false)
    })
  };
```
- `catch`の処理ではerrorのstateをtrueにする
- 関数に入った段階でLoadingのstateをtrueにして  
  finallyでLoadingのstateをfalseにすることでLoadingを実現できる

#### customHookを使用する

<details>
  <summary>customHook使用ver</summary>
  
  ```ts
  export const useAllUsers = () => {
  const [userProfile, setUserProfile] = useState<UserProfile[]>([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(false);

  const getUsers = () => {
    setLoading(true);
    setError(false);

    axios
      .get<User[]>("https://jsonplaceholder.typicode.com/users")
      .then((res) => {
        const data = res.data.map((user) => ({
          id: user.id,
          name: `${user.name} (${user.username})`,
          email: user.email,
          address: `${user.address.city}${user.address.suite}${user.address.street}`
        }));
        setUserProfile(data);
      })
      .catch(() => {
        setError(true);
      })
      .finally(() => {
        setLoading(false);
      });
  };

  return { getUsers, userProfile, loading, error };
};
  ```
  
  ```ts
  export default function App() {
  const { getUsers, userProfile, loading, error } = useAllUsers()

  const onClickFetchUser = () => {
    getUsers()
  }

  return (
    <div className="App">
      <button onClick={onClickFetchUser}>データ取得</button>
      <br />
      {error ? (
        <>
          <p style={{ color: "red" }}>データの取得に失敗しました</p>
        </>
      ) : loading ? (
        <p>Loading....</p>
      ) : (
        <>
          {userProfile.map((user) => (
            <UserCard user={user} key={user.id} />
          ))}
          <UserCard user={user} />
        </>
      )}
    </div>
  );
}
  ```
</details>

**カスタムフックの使用によって超簡潔にまとまって、コンポネートからロジックを分離することに成功した**

### 簡単なログイン実装
1. ログイン画面でuserIDを入力する
2. そのIDをもとにAPIで絞り込みをする
3. 結果、userが見つかればHome画面に遷移する

```ts
export const useAuth = () => {
  const navigate = useNavigate()
  const [loading, setLoading] = useState(false)

  const login = useCallback((id: string) => {
    setLoading(true)

    axios.get<User[]>(`https://jsonplaceholder.typicode.com/users/${id}`)
    .then((res) => {
      if (res.data) {
        navigate("/home")
      } else {
        alert('user does not exists')
      }
    })
    .catch(() => {
      alert("can't login")
    })
    .finally(() => {
      setLoading(false)
    })
  }, [])

  return { login, loading }
}
```
