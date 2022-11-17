# kmp字符串

## 暴力做法

S[N]是原串(较长的)，p[M]是要匹配的串(较短的)

如：`S[] = ababa，p[] = aba`

暴力：分别从头枚举原串和要匹配的串，对应字符进行匹配。

```C++
for (int i = 1; i <= n; i ++ ) // 枚举原串
{
    bool flag = true; // 定义匹配标识，true表示匹配成功
    for (int j = 1; j <= m; j ++ ) // 枚举要匹配的串
    {
        if (s[i + j - 1] != p[j]) // 每次i指针和j指针都向后移动同样的位数
        {
            flag = false;
            break;
        }
    }
}
```

![image-20221113201707865](https://cdn.jsdelivr.net/gh/Lx001T/my-imgs/jq2022/image-20221113201707865.png)

**更新方法**

暴力做法的更新方法比较笨，浪费了很多有用信息。

匹配字符串每次向右移动一个单位，每次都要从头开始**重新匹配**！

![image-20221113201740085](https://cdn.jsdelivr.net/gh/Lx001T/my-imgs/jq2022/image-20221113201740085.png)

因此优化的时候一个思路就是优化**模式字符串的移动**

## 如何优化

### 求next数组

将模板字符串的移动进行优化，可以将已经匹配成功的信息预处理出来。`将模式串p[]中的每一个点都预处理出来后缀和前缀相等的长度最大值`，这个前后缀相等的数组就叫做`next[]数组`

![image-20221113205530914](https://cdn.jsdelivr.net/gh/Lx001T/my-imgs/jq2022/image-20221113205530914.png)

从`下标为1`的位置开始的一串字符串和`以i为终点`，`长度为j`的字符串相等。

**注意：这个next[]数组是只对于模式串p[]来说的！**

### next数组实例模拟

![image-20221113215653774](https://cdn.jsdelivr.net/gh/Lx001T/my-imgs/jq2022/image-20221113215653774.png)

### 匹配过程模拟

由上述分析可以得到：

1. 每次`i是和j+1进行匹配的`，错一位匹配。
2. 样例的next[]数组已经算好了，如下

![image-20221113222009407](https://cdn.jsdelivr.net/gh/Lx001T/my-imgs/jq2022/image-20221113222009407.png)

# 代码如下

```C++
#include <iostream>

using namespace std;

const int N = 100010, M = 1000010;

int n, m;
char p[N], s[M];
int ne[N];

int main()
{
    cin >> n >> p + 1 >> m >> s + 1;
    
    // 求next[]的过程
    for (int i = 2, j = 0; i <= n; i ++ )
    {
        while (j && p[i] != p[j + 1]) j = ne[j];
        if (p[i] == p[j + 1]) j ++ ;
        ne[i] = j;
    }
    
    // kmp 匹配过程
    for (int i = 1, j = 0; i <= m; i ++ )
    {
        while (j && s[i] != p[j + 1]) j = ne[j];
        if (s[i] == p[j + 1]) j ++ ;
        if (j == n)
        {
            printf("%d ", i - n);
            j = ne[j];
        }
    }
    
    return 0;
}
```

## 代码解析

**变量解释**

```C++
int n, m; // n是模式串p[]的长度，m是原串s[]的长度
char p[N], s[M]; // 存储模式串和原串的数组
int ne[N]; // 存储next值的数组
```

**求next**

```C++
// 求next[]的过程
for (int i = 2, j = 0; i <= n; i ++ ) // next[1]必等于0，具体参考上述求next数组的过程。由于求next数组是只根据模式串来求，所以到循环模式的长度n即可
{
    while (j && p[i] != p[j + 1]) j = ne[j]; // j没有跳到开头，并且没有匹配成功，j继续跳转
    if (p[i] == p[j + 1]) j ++ ; // 匹配成功之后，继续向右匹配
    ne[i] = j; // 记录一下next数组的zhi
}
```

**kmp匹配过程**

```C++
// kmp 匹配过程
for (int i = 1, j = 0; i <= m; i ++ )
{
    while (j && s[i] != p[j + 1]) j = ne[j]; // 如果匹配不成功，j向后退一步即求一下next[j]
    if (s[i] == p[j + 1]) j ++ ; // 匹配成功之后，继续向右匹配
    if (j == n) // 匹配成功
    {
        printf("%d ", i - n);
        j = ne[j]; // 匹配成功之后，进行下一次匹配，看最多能到哪里（多次匹配）
    }
}
```

![image-20221113223434620](https://cdn.jsdelivr.net/gh/Lx001T/my-imgs/jq2022/image-20221113223434620.png)
