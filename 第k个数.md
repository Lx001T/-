# 题目描述

给定一个长度为 n 的整数数列，以及一个整数 k，请用快速选择算法求出数列从小到大排序后的第 k 个数。

**输入格式**
第一行包含两个整数 n 和 k。

第二行包含 n 个整数（所有整数均在 1∼109 范围内），表示整数数列。

**输出格式**
输出一个整数，表示数列的第 k 小数。

**数据范围**
1≤n≤100000,
1≤k≤n

**输入样例：**
```
5 3
2 4 1 5 3
```
**输出样例：**
```
3
```
----------

# 快排+输出第k个数
这是一个菜鸡(本人)的思路，求第k个数就是先把所有数按照顺序排好
再输出第k个数即可
## C++ 代码
```
#include <iostream>

using namespace std;

const int N = 100010;
int n, k;
int q[N];

void quick_sort(int q[], int l, int r)
{
    if (l >= r) return;
    
    int i = l - 1, j = r + 1, x = q[i + j >> 1];
    while(i < j)
    {
        while(q[ ++ i] < x);
        while(q[ -- j] > x);
        if (i < j) swap(q[i], q[j]);
    }
    
    quick_sort(q, l, j);
    quick_sort(q, j + 1, r);
}

int main()
{
    cin >> n >> k;

    for (int i = 0; i < n; i ++ ) cin >> q[i];
    
    quick_sort(q, 0, n - 1);
    
    cout << q[k - 1] << ' ';
    
    return 0;
}
```

----------

# 快选
## C++ 代码
```
#include <iostream>

using namespace std;

const int N = 100010;
int n, k;
int q[N];

int quick_sort(int l, int r, int k)
{
    if(l == r) return q[l];

    int i = l - 1, j = r + 1, x = q[(l + r) / 2];

    while(i < j)
    {
        while(q[ ++ i] < x);
        while(q[ -- j] > x);
        if(i < j) swap(q[i], q[j]);
    }
    //做总结，以上为快排代码。具体解析在快速排序题解中。
    int sl = j - l + 1; //此处定义sl，为左边区间的长度，用来衡量k的大小
    if(k <= sl) return quick_sort(l, j, k); //如果k的长度小于sl（sl已经排序好了），那么k一定在sl中

    return quick_sort(j + 1, r, k - sl); //否则在右区间中，注意k的真实长度得减去一个左区间的长度。
}

int main()
{
    cin >> n >> k;
    for(int i = 0; i < n; i ++) cin >> q[i];

    cout << quick_sort(0, n - 1, k) << endl;

    return 0;
}
```
## 核心代码剖析
这是yxc提供的思路
**思路描述**
将区间分为左右两部分(快排的时候已经自动分好了，不用手动分)
比较k与左区间的长度(记为sl)的大小。
       1. if(k < sl) 那么第k个数一定在左区间内，所以只需要递归处理左半区间即可。
       2. if(k > sl) 那么第k个数一定在右区间内，所以只需要递归处理右半区间即可。

**注意：**
如果k在右半区间，递归处理的时候，要注意区间的**左端点**
左端点是： k的长度 - 左半区间的长度

```
    int sl = j - l + 1;
    if(k <= sl) return quick_sort(l, j, k); 
    return quick_sort(j + 1, r, k - sl);
```
```
sl = j - l + 1;
// j是左区间的右端点，l是0，所以最后得+1
```