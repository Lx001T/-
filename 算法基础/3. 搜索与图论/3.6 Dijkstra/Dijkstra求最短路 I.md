![image-20221215220221068](https://cdn.jsdelivr.net/gh/Lx001T/my-imgs/jq2022/image-20221215220221068.png)

# 朴素Dijkstra

## 算法步骤

1. `初始化距离`：$dist[1] = 0，dist[i] = +\infty $

   第一个点到起始的距离为0，其他所有点的距离初始化为正无穷(用一个很大的数表示正无穷)。

2. `迭代求每个点的最短距离`

   ```C++
   // S表示当前已确定最短距离的点的距离
   for i : 1 ~ n
       t <- 不在S中的距离最近的点  // 取出符号条件的点，赋给t
       S <- t   // 将t加到集合S中
       用t更新其他点的距离 // 更新条件：某个点x，若从1到t，加从t到x的距离小于从1到x的距离，进行更新
   ```

   

## 样例模拟

![image-20221215224820508](https://cdn.jsdelivr.net/gh/Lx001T/my-imgs/jq2022/image-20221215224820508.png)

迭代完成之后，每个点到起点的距离就确定了。

朴素Dijkstra由于边数很多，所以是稠密图，用邻接矩阵存储。稀疏图用邻接表存储。

# 代码如下

```C++
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 510;

int n, m;
int g[N][N]; // 稠密图矩阵
int dist[N]; // 从1号点走到每个点的距离
bool st[N]; // 表示每个点的最短路是否已确定

int dijkstra()
{
    memset(dist, 0x3f, sizeof dist); // 将第一个点到所有点的距离初始化为正无穷
    dist[1] = 0;
    
    for (int i = 0; i < n; i ++ )
    {
        int t = -1; // 表示没有确定最短路
        for (int j = 1; j <= n; j ++ ) // 遍历所有点
            if (!st[j] && (t == -1 || dist[t] > dist[j]))
                t = j;
                
        st[t] = true;
        
        for (int j = 1; j <= n; j ++ )
            dist[j] = min(dist[j], dist[t] + g[t][j]);
    }
    
    if (dist[n] == 0x3f3f3f3f) return -1;
    return dist[n];
}

int main()
{
    scanf("%d%d", &n, &m);
    
    memset(g, 0x3f, sizeof g);
    
    while (m -- )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        g[a][b] = min(g[a][b], c); // 解决重边问题：两点之间存在多条边，取距离最短的那条
    }
    
    int t = dijkstra();
    
    printf("%d\n", t);
    
    return 0;
}
```

