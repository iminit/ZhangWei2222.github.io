---
title: 服务器部署 node 环境
date: 2020-04-28 20:23:47
categories:
- 服务器
comments: false
---



### 安装

1. 安装 epel 源

    ```bash
    yum install epel-release
    ```

2. 安装 node npm

    ```bash
    yum install npm nodejs
    ```

3. 不断 y ，y 按下去，然后检查是否成功

    ```bash
    node -v
    ```

除了可以使用 yum 安装软件，还可以使用 wgdt 再tar 安装 [xshell 6+centos下安装node环境](https://blog.csdn.net/themagickeyjianan/article/details/90417402)（较麻烦）



### 配置环境变量

```bash
# 1. 查看当前的环境变量配置
env 
# 2. 修改/etc/profile文件，在末尾添加以下内容
export NODE_HOME=/usr/local/node  # Node 所在路径，可以使用 whereis node 查看
export PATH=$NODE_HOME/bin:$PATH
# 3. 执行命令
source /etc/profile  
# 4. 重启后，即可生效
```

