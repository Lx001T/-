spfa算法实际上是对bellman-ford算法进行优化

算法伪代码：

![image-20221230114944188](https://cdn.jsdelivr.net/gh/Lx001T/my-imgs/jq2022/image-20221230114944188.png)

**用更新过的边再去更新别人。**

代码如下：

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

int spfa()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;
    
    queue<int> q;
    q.push(1);
    st[1] = true; // 这个点是否在队列中，防止存储重复的点
    
    while (q.size())
    {
        int t = q.front();
        q.pop();
        
        st[t] = false;
        
        for (int i = h[t]; i != -1; i = ne[i])
        {
            int j = e[i];
            if (dist[j] > dist[t] + w[i])
            {
                dist[j] = dist[t] + w[i];
                if (!st[j])
                {
                    q.push(j);
                    st[j] = true;
                }
            }
        }
    }
    
    if (dist[n] == 0x3f3f3f3f) return -0x3f3f3f3f;
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
    
    int t = spfa();
    
    if (t == -0x3f3f3f3f) puts("impossible");
    else printf("%d\n", t);
    
    return 0;
}
```

