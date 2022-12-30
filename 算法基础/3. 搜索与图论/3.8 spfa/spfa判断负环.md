dsit[x]: 表示当前从1~x的最短距离

cnt[x]: 当前最短路的边数

![image-20221230151403910](https://cdn.jsdelivr.net/gh/Lx001T/my-imgs/jq2022/image-20221230151403910.png)

如果某个`cnt[x]>=n`，表示从1~x至少经过n条边，则至少经过n+1个点。

但是总共只有n个点，说明路径上存在环。

如果要经过这个环，说明经过这个环之后，权重会变小，说明这个环是负环。

**代码如下：**

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
int dist[N], cnt[N];
bool st[N];

void add(int a, int b, int c)
{
    e[idx] = b; w[idx] = c; ne[idx] = h[a]; h[a] = idx ++ ; // 在图中添加边
}

int spfa()
{

    queue<int> q;

    for (int i = 1; i <= n; i ++ )
    {
        st[i] = true;
        q.push(i);
    }
    
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
                cnt[j] = cnt[t] + 1;
                
                if (cnt[j] >= n) return true;
                if (!st[j])
                {
                    q.push(j);
                    st[j] = true;
                }
            }
        }
    }
    
    return false;
    
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
    
    if (spfa()) puts("Yes");
    else puts("No");
    
    return 0;
}
```

