# 搜索顺序1

`按照全排列搜索`：每次搜索每一行，判断皇后可以放到哪一列。

全排列参考题解：[排列数字](https://www.acwing.com/solution/content/147228/)

![image-20221109163552854](https://cdn.jsdelivr.net/gh/Lx001T/my-imgs/jq2022/image-20221109163552854.png)

**注意剪枝！**

1. 可以将全部全排列列出来，再判断每一个排列是否可行。
2. 在搜素每一行的时候，判断排列是否可行。如果不可行，就不用继续进行排列了，直接开始回溯。

## 代码如下

**对搜索顺序1的部分代码解释**

**变量解释**

```C++
int n; // 棋盘是n×n的
char g[N][N]; // 存储棋盘，'.'表示空, 'Q'表示皇后。 queen
bool col[N], dg[N], udg[N]; // 表示列，对角线，反对角线。 由于是遍历每一行，所以不用单独处理每一行
```

**深搜解释**

`for (int i = 0; i < n; i ++ ) puts(g[i]);`，可以看作一行一行输出，这样输出的就是字符串。`g[N][N]`第一维表示行，第二维表示列。

为什么对角线的下标是`dg[u + i]`，为什么反对角线的下标是`udg[n - u + i]`

按照我的理解，这个偏移量的概念和补码的概念类似。以时钟问题为例，假如当前是4点，如果想将时针调到8点，可以顺时针转4个点，或者逆时针转8个点。顺时针、逆时针的点数之和是一个周期12。

![image-20221109172310188](https://cdn.jsdelivr.net/gh/Lx001T/my-imgs/jq2022/image-20221109172310188.png)

![image-20221109173025458](https://cdn.jsdelivr.net/gh/Lx001T/my-imgs/jq2022/image-20221109173025458.png)

```C++
void dfs(int u)
{
    if (n == u) // 注意此时已经变量到最后一层（最底层），且行数=列数
    {
        for (int i = 0; i < n; i ++ ) puts(g[i]); // 输出每一行，此时每一行就是字符串了
        puts("");
        return;
    }
    
    for (int i = 0; i < n; i ++ ) // 遍历每一行
        if (!col[i] && !dg[u + i] && !udg[n - u + i]) // 列、对角线、反对角线都没有皇后
        {
            g[u][i] = 'Q'; // 放上皇后
            col[i] = dg[u + i] = udg[n - u + i] = true; // 标记放上了
            dfs(u + 1); // 进入下一行(层)
            g[u][i] = '.'; // 现场还原
            col[i] = dg[u + i] = udg[n - u + i] = false; // 现场还原
        }
}
```

**整题代码**

```C++
#include <iostream>

using namespace std;

const int N = 20;

int n;
char g[N][N];
bool col[N], dg[N], udg[N];

void dfs(int u)
{
    if (n == u)
    {
        for (int i = 0; i < n; i ++ ) puts(g[i]);
        puts("");
        return;
    }
    
    for (int i = 0; i < n; i ++ )
        if (!col[i] && !dg[u + i] && !udg[n - u + i])
        {
            g[u][i] = 'Q';
            col[i] = dg[u + i] = udg[n - u + i] = true;
            dfs(u + 1);
            g[u][i] = '.';
            col[i] = dg[u + i] = udg[n - u + i] = false;
        }
}

int main()
{
    cin >> n;
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < n; j ++ )
            g[i][j] = '.';
            
    dfs(0);
    
    return 0;
}
```

# 第二种搜索顺序

**逐个枚举**：假如棋盘是n×n的，对这$n^2$个格子逐个枚举，每一个格子只有两种选择，**放/不放**皇后。从(0,0)枚举到(n,n)即可得到结果。

![image-20221109192606548](https://cdn.jsdelivr.net/gh/Lx001T/my-imgs/jq2022/image-20221109192606548.png)

**搜索顺序如下：**

只要搜索结束，并且皇后数量为n的话，就是一组解。

![image-20221109194415150](https://cdn.jsdelivr.net/gh/Lx001T/my-imgs/jq2022/image-20221109194415150.png)

## 代码如下

```C++
#include <iostream>

using namespace std;

const int N = 20;

int n;
char g[N][N];
bool row[N], col[N], dg[N], udg[N];

void dfs(int x, int y, int s)
{
    if (y == n) y = 0, x ++ ; // 搜到第n列，跳转到下一列
    
    if (x == n) // 满足搜到第n行
    {
        if (s == n) // 并且放了n个皇后，则这是一组解
        {
            for (int i = 0; i < n; i ++ ) puts(g[i]); // 按行打印
            puts("");
        }
        return;
    }
    
    // 不放皇后
    dfs(x, y + 1, s); // 不放皇后直接跳到下一行
    
    // 放皇后
    if (!col[y] && !row[x] && !dg[y - x + n] && !udg[x + y]) // 行列、对角线、反对角线都没有交点
    {
        g[x][y] = 'Q';
        col[y] = row[x] = dg[y - x + n] = udg[x + y] = true;
        dfs(x, y + 1, s + 1);
        g[x][y] = '.';
        col[y] = row[x] = dg[y - x + n] = udg[x + y] = false;
    }
}

int main()
{
    cin >> n;
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < n; j ++ )
            g[i][j] = '.';
            
    dfs(0, 0, 0); // 从坐标(0,0)，皇后数量为0开始搜
    
    return 0;
}
```

