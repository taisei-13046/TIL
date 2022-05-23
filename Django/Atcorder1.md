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

