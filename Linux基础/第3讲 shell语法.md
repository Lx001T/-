# 概论

```shell
# 新建
vim test.sh

# 编写
#! /bin/bash
echo "Hello world!"

# 添加可执行权限
chmod +x test.sh

# 执行
(1) ./test.sh
(2) /home/acs/test.sh  # 绝对路径下执行
(3) ~/test.sh  # 家目录路径下执行
(4) bash test.sh # 解释器执行
```

# 注释

```shell
# 单行注释
# 这是注释
echo "Hello world!" # 这也是注释

# 多行注释
:<<EOF
aaa
bbb
ccc
EOF
# 其中EOF可以换成任意字符
```

# 变量

```shell
# 定义变量
name1="ljq" 或 'ljq' 或 ljq

# 使用变量
echo ${name}

# 只读变量
使用readonly或者declare可以将变量变为只读。
name=ljq
readonly name
declare -r name  # 两种写法均可

# 删除变量
unset可以删除变量。

# 变量类型
自定义变量（局部变量）
子进程不能访问的变量
环境变量（全局变量）
子进程可以访问的变量

自定义变量改成环境变量：
acs@9e0ebfcd82d7:~$ name=yxc  # 定义变量
acs@9e0ebfcd82d7:~$ export name  # 第一种方法
acs@9e0ebfcd82d7:~$ declare -x name  # 第二种方法

环境变量改为自定义变量：
acs@9e0ebfcd82d7:~$ export name=yxc  # 定义环境变量
acs@9e0ebfcd82d7:~$ declare +x name  # 改为自定义变量

# 字符串
字符串可以用单引号，也可以用双引号，也可以不用引号。
区别：
单引号中的内容会原样输出，不会执行、不会取变量；
双引号中的内容可以执行、可以取变量；

name=yxc  # 不用引号
echo 'hello, $name \"hh\"'  # 单引号字符串，输出 hello, $name \"hh\"
echo "hello, $name \"hh\""  # 双引号字符串，输出 hello, yxc "hh"

# 获取字符串长度
name="yxc"
echo ${#name}  # 输出3

# 提取子串
name="hello, yxc"
echo ${name:0:5}  # 提取从0开始的5个字符
```

# 默认变量

在执行shell脚本时，可以向脚本传递参数。`$1是第一个参数，$2是第二个参数，以此类推`。特殊的，$0是文件名（包含路径）。例如：

```shell
# 创建
#! /bin/bash

echo "文件名："$0
echo "第一个参数："$1
echo "第二个参数："$2
echo "第三个参数："$3
echo "第四个参数："$4

# 执行
acs@9e0ebfcd82d7:~$ chmod +x test.sh 
acs@9e0ebfcd82d7:~$ ./test.sh 1 2 3 4
文件名：./test.sh
第一个参数：1
第二个参数：2
第三个参数：3
第四个参数：4
```

# 数组

数组中可以存放多个不同类型的值，只支持一维数组，初始化时不需要指明数组大小。
数组下标从0开始。

```shell
# 两种方式定义数组
array=(1 abc "def" yxc)

array[0]=1
array[1]=abc
array[2]="def"
array[3]=yxc

# 读取数组中某个元素的值
${array[index]}

# 读取整个数组
${array[@]}
${array[*]}

# 数组长度
${#array[@]}
```

# expr

**expr命令用于求表达式的值**，格式为：

`expr` 表达式
表达式说明：

用空格隔开每一项
用反斜杠放在shell特定的字符前面（发现表达式运行错误时，可以试试转义）
对包含空格和其他特殊字符的字符串要用引号括起来
expr会在stdout中输出结果。如果为逻辑关系表达式，则结果为真时，stdout输出1，否则输出0。
expr的exit code：如果为逻辑关系表达式，则结果为真时，exit code为0，否则为1。

**字符串表达式**

`length STRING`
返回STRING的长度
`index STRING CHARSET`
CHARSET中任意单个字符在STRING中最前面的字符位置，下标从1开始。如果在STRING中完全不存在CHARSET中的字符，则返回0。
`substr STRING POSITION LENGTH`
返回STRING字符串中从POSITION开始，长度最大为LENGTH的子串。如果POSITION或LENGTH为负数，0或非数值，则返回空字符串。

```shell
str="Hello World!"

echo `expr length "$str"`  # ``不是单引号，表示执行该命令，输出12
echo `expr index "$str" aWd`  # 输出7，下标从1开始
echo `expr substr "$str" 2 3`  # 输出 ell
```

**整数表达式**

```shell
expr支持普通的算术操作，算术表达式优先级低于字符串表达式，高于逻辑关系表达式。
+ -
加减运算。两端参数会转换为整数，如果转换失败则报错。
* / %
乘，除，取模运算。两端参数会转换为整数，如果转换失败则报错。
() 可以改变优先级，但需要用反斜杠转义

a=3
b=4

echo `expr $a + $b`  # 输出7
echo `expr $a - $b`  # 输出-1
echo `expr $a \* $b`  # 输出12，*需要转义
echo `expr $a / $b`  # 输出0，整除
echo `expr $a % $b` # 输出3
echo `expr \( $a + 1 \) \* \( $b + 1 \)`  # 输出20，值为(a + 1) * (b + 1)
```



