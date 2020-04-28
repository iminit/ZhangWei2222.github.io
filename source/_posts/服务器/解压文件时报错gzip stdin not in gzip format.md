---
title: 解压文件时报错gzip stdin not in gzip format
date: 2020-04-28 21:27:47
categories:
- 服务器
comments: false
---

在服务器解压 MySQL 压缩包时报错

```bash
gzip: stdin: not in gzip format
tar: Child returned status 1
tar: Error is not recoverable: exiting now
```

用的命令为 `tar -zxvf mysql-server-...e.tar`

解决办法：将参数改为 `-xvf`

