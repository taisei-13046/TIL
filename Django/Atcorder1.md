https://atcoder.jp/contests/typical90/tasks/typical90_d
## 典型004: Cross Sum

問題文
H 行 W 列のマス目があります。上から i (1≤i≤H) 行目、左から j (1≤j≤W) 列目にあるマス (i,j) には、整数 A 
i,j が書かれています。 すべてのマス (i,j) (1≤i≤H,1≤j≤W) について、以下の値を求めてください。

入力例
3 3  
1 1 1  
1 1 1  
1 1 1  

回答例

```python
H, W = map(int, input().split())

h = [0]*H
w = [0]*W

A = [[] for i in range(H)]

for i in range(H):
    a = list(map(int, input().split()))
    A[i] = a
    h[i] = sum(a)
    for j in range(W):
        w[j] += a[j]

for i in range(H):
    li = [0]*W
    for j in range(W):
        n = A[i][j]
        li[j] = h[i] + w[j] - n
    print(*li)
```

```python
H,W = map(int,input().split())
#A = list(map(int,input().split()))
A = []
for _ in range(H):
    A.append(list(map(int,input().split())))
P = [0]*H
for i in range(H):
    for j in range(W):
        P[i]  += A[i][j]
Q = [0]*W
for j in range(W):
    for i in range(H):
        Q[j]  += A[i][j]
for i in range(H):
    for j in range(W):
        ans = P[i] + Q[j] - A[i][j]
        print(ans,end=" ")
    print()
```

```python
import copy

H, W = map(int, input().split())
table = []
for i in range(H):
    table.append(list(map(int, input().split())))

row_sum = [sum(row) for row in table]
column_sum = [sum(column) for column in zip(*table)]

ans = copy.deepcopy(table)


for i in range(H):
    for j in range(W):
        ans[i][j] = row_sum[i] + column_sum[j] - ans[i][j]

for li in ans:
    print(' '.join(map(str, li)))
```

### map関数
[Pythonのmap()でリストの要素に関数・処理を適用](https://note.nkmk.me/python-map-usage/)  

`map(int, input().split())`

atcorderでの典型文  

map()の第一引数に適用する関数（呼び出し可能オブジェクト）、第二引数にリストなどのイテラブルオブジェクトを指定する。  

### 二次元配列のinput
```python
table = []
for i in range(H):
    table.append(list(map(int, input().split())))
```

