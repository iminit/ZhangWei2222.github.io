---
title: Linux 下卸载 MySQL 数据库
date: 2020-04-28 21:01:47
categories:
- 服务器
comments: false
---

因为数据库突然崩了，所以有了这么一次卸载的机会，记录一下，记录一下

<!-- more -->

### 卸载

#### 1. 查看当前安装情况

```bash
rpm -qa|grep -i mysql
```



#### 2. 检查 MySQL 服务并关闭服务进程

查看 MySQL 服务状态，根据开机初始化目录下 MySQL 脚本的文件名而定，我这里是 mysqld

```bash
service mysqld status
```

如果正在运行则需要关闭服务

```bash
service mysqld stop
```



#### 3. 删除之前安装的 MySQL

```bash
rpm -ev {包名}
```

**如果提示依赖包错误**，则使用以下命令尝试：

```bash
rpm -ev MySQL-client-5.5.25a-1.rhel5 --nodeps
```

**如果提示错误：**`error: %preun(xxxxxx) scriptlet failed, exit status 1` ，则使用以下命令尝试：

```bash
rpm -e --noscripts {包名}
```

**如果提示错误：**`mysql-community-common(x86-64) >= 5.7.9 is needed by mysql-community-libs-5.7.21-1.el7.x86_64` ，这个时候需要将已经安装的 rpm包 卸载（依赖问题）



#### 4. 查找 mysql 的安装目录并彻底删除

查找跟 mysql 相关的目录

```bash
find / -name mysql 
```

删除查找出来的目录

```bash
rm -rf '目录'
```



#### 5. 删除 mysql 配置文件

删除 /etc/my.cnf 文件

```bash
rm -rf /etc/my.cnf
```

删除 /etc/init.d/ 下跟 mysql 有关的全部文件，一般包括 mysql 文件或 mysqld 文件

```bash
rm -rf /etc/init.d/mysqld
```

如果存在mysql文件则删除

```bash
rm -rf/etc/init.d/mysql
```



#### 6. 删除mysql用户及用户组

查看 MySQL 用户及用户组

```bash
id mysql
```

删除MySQL用户及用户组

```bash
userdel mysql
```

**如果使用 userdel 无法删除用户，如下报错**

```bash
root@ per # userdel -r mysql
userdel： user mysql is currently used by process 1748

root@ per # groupdel mysql
groupdel：不能移除用户“mysql”的主组
# 删除该用户就提醒用户当前在进程运行，删除他的组也报错。
```

**则可使用 vipw 命令**

```bash
root@ per# vipw
# 找到之前创建的用户，用dd删除那行（记得保存：wq or ：x）。

root@per# vipw -s
# 找到那个用户所属组，也dd干掉即可（记得保存：wq or ：x）
# 使用vipw -s的原因只有一个，就是必须保证数据的一致性，不然可能会造成系统崩溃等问题。
```



#### 7. 再次查找机器是否安装 MySQL

```bash
rpm -qa|grep -i mysql
```

无结果，则代表彻底卸载完毕




### 参考文章
[Linux下MySQL卸载和安装图文教程](https://www.jb51.net/article/129294.htm)

[Linux->解决用userdel删除不掉用户的问题](https://www.cnblogs.com/taomylife/p/7234892.html)

[Linux下彻底卸载mysql详解](https://www.cnblogs.com/javahr/p/9245443.html)