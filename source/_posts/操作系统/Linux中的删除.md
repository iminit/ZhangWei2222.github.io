---
title:  Linux中的删除
date: 2019-11-07 17:03:37
categories:
- 操作系统
tags:
- Linux
comments: false
---



- -r 就是向下递归，不管有多少级目录，一并删除
- -f 就是直接强行删除，不作任何提示的意思

```shell
# 删除文件夹实例：
$ rm -rf /var/log/httpd/access
# 将会删除/var/log/httpd/access目录以及其下所有文件、文件夹

# 删除文件使用实例：
$ rm -f /var/log/httpd/access.log
# 将会强制删除/var/log/httpd/access.log这个文件
```