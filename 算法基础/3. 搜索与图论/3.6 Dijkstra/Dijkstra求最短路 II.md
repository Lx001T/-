朴素Dijkstra算法参考：[Dijkstra求最短路 I](https://www.acwing.com/file_system/file/content/whole/index/content/7478992/)

# 堆优化Dijkstra

使用堆优化有两种方法：

1. 手写堆
2. 用优先队列

这里采用优先队列。

由于是用稀疏图，所有用邻接表存储。

```C++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <queue>

using namespace std;

typedef pair<int, int> PII;

const int N = 150010;

int n, m;
int h[N], w[N], e[N], ne[N], idx; // 邻接表使用的数据结构，w[]存储权重
int dist[N];
bool st[N];

void add(int a, int b, int c)
{
    e[idx] = b; w[idx] = c; ne[idx] = h[a]; h[a] = idx ++ ; // 在图中添加边
}

int dijkstra()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;
    
    priority_queue<PII, vector<PII>, greater<PII>> heap; // 小根堆
    heap.push({0, 1}); // 先把第一个点放入堆中，距离为0，点数为1
    
    while (heap.size())
    {
        auto t = heap.top(); // 取出堆中最小的点，即本题中距离最短的
        heap.pop(); // 将这个点出堆
        
        int ver = t.second, distance = t.first; // 取出距离和点数
        
        if (st[ver]) continue; // 如果之前用过这个点，跳过找下一个
        st[ver] = true;
        
        for (int i = h[ver]; i != -1; i = ne[i]) // 遍历这个点的所有临边
        {
            int j = e[i]; // 将每个点的值取出
            if (dist[j] > distance + w[i]) // 如果满足跟新条件
            {
                dist[j] = distance + w[i]; // 将其更新
                heap.push({dist[j], j});  // 将其插进堆中
            }
        }
    }
    
    
    if (dist[n] == 0x3f3f3f3f) return -1;
    return dist[n];
}

int main()
{
    scanf("%d%d", &n, &m);

    memset(h, -1, sizeof h);
    
    while (m -- )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        add(a, b, c);
    }
    
    int t = dijkstra();
    
    printf("%d\n", t);
    
    return 0;
}

```

