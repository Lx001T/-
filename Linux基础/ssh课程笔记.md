获得一个自己的服务器。

```ssh
homework 4 getinfo
```

# ssh登录

作用：用ssh登录到某一个服务器中，进行开发

![image-20230323160921521](../../../AppData/Roaming/Typora/typora-user-images/image-20230323160921521.png)

登录方法：

```
ssh user@hostname
```

`user`: 用户名
`hostname`: IP地址或域名

![image-20230323162028123](../../../AppData/Roaming/Typora/typora-user-images/image-20230323162028123.png)

## 配置代替ip地址

![image-20230323162123690](../../../AppData/Roaming/Typora/typora-user-images/image-20230323162123690.png)

![image-20230323162151001](../../../AppData/Roaming/Typora/typora-user-images/image-20230323162151001.png)

## 配置免密登录

```
ssh-keygen
将里面的公钥复制到要免密登录服务器的~/.ssh/authorized_keys中

一键生成
ssh-copy-id myserver
```

# scp

```
# 将source路径下的文件复制到destination中
scp source1 source2 destination
```

![image-20230323164002912](../../../AppData/Roaming/Typora/typora-user-images/image-20230323164002912.png)

# 配置自己的服务器

```
使用scp配置其他服务器的vim和tmux
scp ~/.vimrc ~/.tmux.conf myserver:
```