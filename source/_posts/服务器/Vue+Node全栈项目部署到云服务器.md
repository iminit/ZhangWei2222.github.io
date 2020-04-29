---
title: Vue+Node全栈项目部署到云服务器
date: 2020-04-23 11:09:47
categories:
- 服务器
comments: false
---

### 前言

本次毕业项目实现了全栈开发，使用到的技术栈为：Vue + Vant + Node.js + Koa2 + MySQL + TypeScript，为了把项目部署到云服务器，还是折腾了一段时间，接下来记录一下最终摸索出来的部署过程。

<!-- more -->



### 服务器和目录介绍

阿里云服务器，CentOS 7.3

```json
Youth-Hostel
├── client  // 前端 Vue 工程
├── db      // 后端 Node 工程
├── logs    // 后端日志
├── package-lock.json
├── package.json
├── public  // 上传头像存放处，存放地址已更改至服务器
└── README.md
```



### 完成本地调试

1. 安装 concurrently

   ```bash
   npm i concurrently --save
   ```

2. 修改 `package.json` 的 `script`

   此时，在本地调试时，只运行一条命令 **npm run dev**，就可以同时启动前后端工程项目

    ```json
    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1",
        "client": "npm start --prefix client",
        "server": "nodemon ./db/index.ts",
        "dev": "concurrently \"npm run server\" \"npm run client\""
    }
    ```


#### 本地修改文件

1. 修改 `client->src->router->index.js` 的 mode 值

    ```js
    const router = new Router({
        mode: 'hash', 
        routes
    })
    ```

2. 修改 `config/index.js` 的 `assetsPublicPath` 为 **./**（注意一定是 **build** 里面的，上面 **dev** 中也有这个配置）
3. 打包成 dist文件，然后扔到服务器中



### 服务器部署

#### 配置 Nginx

1. 服务器需要安装 Nginx，安装后通过 `nginx -v` 查看是否成功

2. 修改 Nginx 的配置文件，前面在 [部署项目到服务器](http://super-wei.top/2017/08/22/%E6%9C%8D%E5%8A%A1%E5%99%A8/%E9%83%A8%E7%BD%B2%E9%A1%B9%E7%9B%AE%E5%88%B0%E6%9C%8D%E5%8A%A1%E5%99%A8/) 文章中有介绍，接下来需要增加修改

   ```shell
   server {
           listen       80 default_server; # 项目跑的端口
           listen       [::]:80 default_server;
           server_name   _; # 当前服务器ip
           root         /var/www;
   
           # Load configuration files for the default server block.
           include /etc/nginx/default.d/*.conf;
   
           location / {}
   
           location /api { # 重点：请求跨域时配置端口转发
             proxy_pass http://101.133.132.172:4442/api; # 重点：转发地址
           }
   
           error_page 404 /404.html;
               location = /40x.html {
           }
   }
   ```

   如果 Nginx 不进行跨域的配置，虽然项目部署了，但是服务也访问不到

#### 上传文件

1. 把 `client->dist` 里面的文件扔到服务器配置的地址 `/var/www`，个人使用 FileZilla 软件
2. 把 `db` 整个文件夹扔到  `/var/www`
3. 在 `/var/www` 创建 `logs 和 public` 目录，放日志和头像等文件
4. 将本地项目的根目录 `package.json` 扔到 `/var/www`
5. 使用 xshell 软件连接服务器，进入到 `/var/www` 目录中，执行 `npm install `（如果权限不够，执行命令： `sudo npm install`）

#### 云服务器开放端口

因为后端用到的端口是 4442，所以要在云服务器上开放 4442 端口。需要前往阿里云服务器管理页面，进入防火墙，添加规则：

- 应用类型：自定义
- 协议：TCP
- 端口号：4442

#### 安装 pm2

服务器上使用 node 命令启动项目时，如果我们退出 Xshell 时，进程就会关闭，无法再访问到项目。因此，为了解决这个问题需要用到 pm2。pm2 是一个带有负载均衡功能的 Node 应用的进程管理器，可以保证进程永远都活着，0秒的重载。

前面 [服务器安装 pm2 管理工具](http://super-wei.top/2020/04/28/%E6%9C%8D%E5%8A%A1%E5%99%A8/%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%AE%89%E8%A3%85%20pm2%20%E7%AE%A1%E7%90%86%E5%B7%A5%E5%85%B7/) 文章有介绍



### 测试

在浏览器地址栏输入云服务器公网ip和端口号进行测试



### 参考文章

[vue3+node全栈项目部署到云服务器](https://www.cnblogs.com/FHC1994/p/11771706.html)

