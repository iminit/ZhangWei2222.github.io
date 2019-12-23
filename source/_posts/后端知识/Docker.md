---
title: Docker
date: 2019-12-17 17:12:37
categories:
- 后端知识
tags:
comments: false
---

## Docker是什么

一个开源的应用容器引擎，可从[Docker Hub](https://hub.docker.com/)里下载需要的镜像，比如：mongoDB，使用一句简单的命令，就可以在Docker跑起来，类似于虚拟机。

<!--more-->

可使用KBS管理Docker，比如Docker挂了，属于自动化运维的范畴，因为比较难，先只做了解。



### 与虚拟机的区别

一个很典型的区别：Docker可以实现资源隔离，分多少内存就只用多少，不会吃其他地方的内存；而虚拟机即使你分了固定的内存，一旦内存泄漏，还是会吃其他地方的内存，然后卡死。



## Mac安装

```dockerfile
$ brew cask install docker

==> Creating Caskroom at /usr/local/Caskroom
==> We'll set permissions properly so we won't need sudo in the future
Password:          # 输入 macOS 密码
==> Satisfying dependencies
==> Downloading https://download.docker.com/mac/stable/21090/Docker.dmg
######################################################################## 100.0%
==> Verifying checksum for Cask docker
==> Installing Cask docker
==> Moving App 'Docker.app' to '/Applications/Docker.app'.
&#x1f37a;  docker was successfully installed!
```

在载入 Docker app 后，点击 Next，可能会询问你的 macOS 登陆密码，你输入即可。之后会弹出一个 Docker 运行的提示窗口，状态栏上也有有个小鲸鱼的图标（![img](https://www.runoob.com/wp-content/uploads/2018/01/1515480613-2248-whale-x.png)）。



## 例子：安装MongoDB镜像

### 1. 拉取镜像

先去[Docker Hub](https://hub.docker.com/)搜索镜像，可以根据`Tags`选择版本，会有命令提示

```dockerfile
docker pull mongo
```

### 2. 查看本地镜像

```
docker images
```

### 3. 运行容器

例如：mongoDB

```bash
$ docker run -itd --name mongo -p 44:27017 mongo --auth
ee91398c42556c3409fb8e69fc9df52b56678a930e28e65a40ae84b7eca68d53

# 假如提示：
docker: Error response from daemon: Conflict. The container name "/mongo" is already in use by container "c85def91c800e8dd8d3d05a858cffa0228d41db43651ebbbac4ecd853983afd7". You have to remove (or rename) that container to be able to reuse that name.

# 使用rm删除容器
$ docker rm mongo
```

- **-p 27017:27017** ：映射容器服务的 44 端口到宿主机的 27017 端口。外部可以直接通过 宿主机 ip:27017 访问到 mongo 的服务
- **--auth**：需要密码才能访问容器服务

### 4. 安装成功

通过`docker ps`可以查看容器的运行信息

```bash
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                   NAMES
ee91398c4255        mongo               "docker-entrypoint.s…"   About a minute ago   Up About a minute   0.0.0.0:44->27017/tcp   mongo
```



## Dockerfile 创建镜像

> 用来创建镜像的文本内容，文本内容包含一条条构建镜像所需的指令和说明

### 简单的案例
```shell
$ mkdir Dockerfile
$ cd Dockerfile/
$ vi Dockerfile/

# 以下是内容
FROM nginx
RUN echo '<h1>hello, docker!</h1>' > /usr/share/nginx/html/index.html

$ docker build -t nginx:test .
# 查看新建的镜像
$ docker images
```

### 常用指令

![img](https://img2018.cnblogs.com/blog/450977/201905/450977-20190512115951746-136143052.png)

- RUN 在 `docker build`时运行
- CMD 在`docker run` 时运行

```shell
# 基于centos镜像
FROM centos

# 维护人的信息
MAINTAINER The CentOS Project <303323496@qq.com>

# 安装httpd软件包
RUN yum -y update
RUN yum -y install httpd

# 开启80端口
EXPOSE 80

# 复制网站首页文件至镜像中web站点下
ADD index.html /var/www/html/index.html

# 复制该脚本至镜像中，并修改其权限
ADD run.sh /run.sh
RUN chmod 775 /run.sh

# 当启动容器时执行的脚本文件
CMD ["/run.sh"]
```

Dockerfile结构大致分为四个部分：

1. 基础镜像信息
2. 维护者信息
3. 镜像操作指令
4. 容器启动时执行指令

更详细的介绍，参考这篇文章：[你必须知道的Dockerfile](https://www.cnblogs.com/edisonchou/p/dockerfile_inside_introduction.html)



## Docker常用命令

```dockerfile
docker build -t friendlyname .  # 使用此目录的“Dockerfile”创建镜像

docker run -p 4000:80 friendlyname  # 将4000 端口映射到80端口

docker run -d -p 4000:80 friendlyname         # 后台运行"friendlyname" 镜像并将4000 端口映射到80端口

docker ps                                 # 查看我们正在运行的容器

docker stop <hash>                     # 停止指定的容器

docker ps -a           # 查看所有的容器，包括没有运行的容器

docker kill <hash>                   # 强制关闭指定的容器

docker rm <hash>              # 删除指定的容器

docker rm $(docker ps -a -q)           # 删除所有容器 

docker images -a                               # 查看所有镜像

docker rmi <imagename>            # 删除指定的镜像

docker rmi $(docker images -q)             # 删除所有的镜像

docker login             # 使用您的 Docker 凭证登录此 CLI 会话

docker tag <image> username/repository:tag  # 标记 <image> 以上传到镜像库

docker push username/repository:tag            # 将已标记的镜像上传到镜像库

docker run username/repository:tag                   # 运行镜像库中的镜像

docker info                              # 查看Docker信息
```

