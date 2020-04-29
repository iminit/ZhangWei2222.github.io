---
title: Vue 项目部署到 Nginx
date: 2020-04-23 10:52:47
categories:
- 服务器
comments: false
---

介绍 Vue 项目部署到 服务器的步骤

<!-- more -->

### Vue 项目打包

1. 修改 `config/index.js` 的 `assetsPublicPath` 为 **./**（注意一定是 **build** 里面的，上面 **dev** 中也有这个配置）
2. 打包成 dist文件，然后扔到服务器中



### 配置 Nginx

1. 服务器需要安装 Nginx，安装后通过 `nginx -v` 查看是否成功

2. 修改 Nginx 的配置文件，前面在 部署项目到服务器 文章中有介绍，接下来需要增加修改

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



### 开放端口

阿里云的 80 端口好像现在默认开了？上次没有配置却成功访问了，如果之后发现没有开，再更改此条信息。

最后需要重启 Nginx ：

```bash
sudo service nginx restart
```



### 参考文章

[vue项目部署到nginx](https://blog.csdn.net/weixin_42565137/article/details/90577950)