# Trie树

Trie树：用于高效地`存储`和`查找`字符串集合的数据结构

## Trie树的存储

![image-20221115220927430](https://cdn.jsdelivr.net/gh/Lx001T/my-imgs/jq2022/image-20221115220927430.png)

**为什么在结尾要打标记？**

在上图基础之上，如果还有一个字符串`abc`要插入

**如果不打标记，就不知道还有字符串abc**

打标机意味着：以这个标记结尾的是有一个单词的！

![image-20221115221135382](https://cdn.jsdelivr.net/gh/Lx001T/my-imgs/jq2022/image-20221115221135382.png)

## Trie树的查找

查找步骤;

**逐个匹配要查找的每个单词**

1. `匹配成功`，查看在匹配的单词结尾是否存在标记
   1. `有标记`：匹配成功，存在此单词
   2. `无标记`：匹配失败，不存在此单词
2. `匹配失败`：不存在此单词

![image-20221115221836352](https://cdn.jsdelivr.net/gh/Lx001T/my-imgs/jq2022/image-20221115221836352.png)

# 代码如下

```C++
#include <iostream>

using namespace std;

const int N = 100010;

int son[N][26], cnt[N], idx;
char str[N];

void insert(char str[])
{
    int p = 0;
    for (int i = 0; str[i]; i ++ )
    {
        int u = str[i] - 'a';
        if (!son[p][u]) son[p][u] = ++ idx;
        p = son[p][u];
    }
    
    cnt[p] ++ ;
}

int query(char str[])
{
    int p = 0;
    for (int i = 0; str[i]; i ++ )
    {
        int u = str[i] - 'a';
        if (!son[p][u]) return 0;
        p = son[p][u];
    }
    
    return cnt[p];
}

int main()
{
    int n;
    cin >> n;
    while (n -- )
    {
        char op[2];
        cin >> op >> str;
        if (op[0] == 'I') insert(str);
        else cout << query(str) << endl;
    }
    
    return 0;
}
```

## 代码解析

**变量解释**

```C++
int son[N][26], cnt[N], idx;
// son[N][26]存储的是每个节点的儿子字符，因为题目给的是26个小写字母，所以第二维开26个
// cnt[N]存储的是以每个字符结尾的单词出现的次数
// idx表示现在处理到了什么位置
char str[N]; // 表示输入的单词
```

**Trie树存储(插入)**

```C++
void insert(char str[])
{
    int p = 0; // 根节点
    for (int i = 0; str[i]; i ++ ) // 遍历单词的每个字符
    {
        int u = str[i] - 'a'; // 将单词'a~z'映射到'0~25'
        if (!son[p][u]) son[p][u] = ++ idx; // 如果下一个路径不存在这个字符，创建一个路径
        p = son[p][u]; // 走过去
    }
    
    cnt[p] ++ ; // 把最后一次走到的字符“打标记” (把以此字符结尾的单词出现的次数+1)
}
```

**Trie树查找**

```c++
int query(char str[])
{
    int p = 0;
    for (int i = 0; str[i]; i ++ )
    {
        int u = str[i] - 'a';
        if (!son[p][u]) return 0; // 不存在，直接返回
        p = son[p][u]; // 存在就走过去
    }
    
    return cnt[p]; // 返回以此字符结尾的单词出现的次数
}
```

**处理输入输出**

```C++
while (n -- )
{
    char op[2];
    cin >> op >> str;
    if (op[0] == 'I') insert(str);
    else cout << query(str) << endl;
}
```

为什么是`char op[2]`？

可能是因为输入的有空白字符，如`scanf("\n%s", &op[0])`

`strlen()函数以’\0’字符判断字符串结束，输入的时候会把这个’\0’字符存储在op[1]`

```C++
// 直接用string处理op也行，但这样好像满了一点，4ms？
while (n -- )
{
    string op;
    cin >> op >> str;
    if (op == "I") insert(str);
    else cout << query(str) << endl;
}
```

![image-20221115230210922](https://cdn.jsdelivr.net/gh/Lx001T/my-imgs/jq2022/image-20221115230210922.png)