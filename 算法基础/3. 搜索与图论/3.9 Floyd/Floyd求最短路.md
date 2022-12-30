# Floyd算法

Floyd算法用来求多源汇最短路。
邻接矩阵`d[i, j]`存储所有的边

```C++
// Floyd算法实现如下, 注意三个迭代顺序
for(int k = 1; k <= n; k ++ )
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= n; j ++ )
            d[i, j] = min(d[i, j], d[i, k] + d[k, j]) // 更新完之后，d[i, j]存的就是i到j最短路的长度
```

原理：

状态表示：`d[k, i, j]从i点出发，只经过1~k这些点，到达j的最短距离。`

`d[k, i, j] = d[k-1, i, k] + d[k-1, k, j]：从1~k-1只加上经过第k个点的距离`

第一维可以优化掉，变成`d[i, j] = d[i, k] + d[k, j]`

# 代码如下

```C++
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 210, INF = 1e9;

int n, m, Q;
int d[N][N];

void floyd()
{
    for (int k = 1; k <= n; k ++ )
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= n; j ++ )
                d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
}

int main()
{
    scanf("%d%d%d", &n, &m, &Q);
    
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= n; j ++ )
            if (i == j) d[i][j] = 0;
            else d[i][j] = INF;
            
    while (m -- )
    {
        int a, b, w;
        scanf("%d%d%d", &a, &b, &w);
        
        d[a][b] = min(d[a][b], w);
    }
    
    floyd();
    
    while (Q -- )
    {
        int a, b;
        scanf("%d%d", &a, &b);
        
        if (d[a][b] > INF / 2) puts("impossible");
        else printf("%d\n", d[a][b]);
    }
    
    return 0;
}
```

