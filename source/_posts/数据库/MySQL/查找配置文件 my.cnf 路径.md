---
title: MySQL-查找配置文件 my.cnf 路径
date: 2020-04-28 20:52:47
categories:
- MySQL
comments: false
---

如果安装时没有做什么修改，则默认路径为 `/etc/my.cnf`
1. 查找 mysqld 的路径

   ```bash
   which mysqld
   # /usr/sbin/mysqld
   ```

2. 敲命令，即可找到 

   ```bash
   /usr/sbin/mysqld --verbose --help |grep -A 1 'Default options'
   ```

   

