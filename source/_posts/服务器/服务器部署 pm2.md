---
title: 服务器部署 pm2
date: 2020-04-28 11:51:47
categories:
- 服务器
comments: false
---

使用 pm2 管理工具，可以维持服务器的后端 node 运行文件，并且能够自动重启，以及查看日志。接下来，介绍其部署的步骤。
<!-- more -->

### 安装

```bash
npm install -g pm2
```



### 部署环境变量

如果以下命令显示 command not found，是因为没有将 pm2 加到环境变量中

```bash
pm2 -v
```

1. 查看 node 目录

   ```bash
   whereis node
   node: /usr/local/bin/node
   ```

2. 进入本地环境变量的路径

    ```bash
    cd /usr/local/bin
    ll
    
    lrwxrwxrwx 1 root root      43 Jul  6 09:49 node -> /usr/nodejs/node-v8.11.3-linux-x64/bin/node
    lrwxrwxrwx 1 root root      42 Jul  6 09:49 npm -> /usr/nodejs/node-v8.11.3-linux-x64/bin/npm
    ```

3. 进入 node 目录

   ```bash
   cd /usr/nodejs/node-v8.11.3-linux-x64/bin
   
   node  npm   pm2
   ```

4. 发现 pm2 ，将其加至本地的环境变量

   ```bash
   ln -s /usr/nodejs/node-v8.11.3-linux-x64/bin/pm2 /usr/local/bin
   ```

5. 检查是否成功

   ```bash
   pm2 -v
   ```

   

### 运行 TypeScript 文件

```bash
pm2 install typescript
pm2 start app.ts
```



### 使用

```bash
# --watch参数是监测 node 文件，如果被更改了会自动重启
pm2 start server.js --watch

# 查看部署的 node 进程
pm2 list

# 重启 pm2
pm2 restart {list里的id号}

# 查看日志
pm2 log
```



### 参考文章

[Linux下使用pm2部署node以及安装后command not found解决](https://blog.csdn.net/d597180714/article/details/82619735)

