---
title:  Npm、Cnpm、Nrm、Nvm
date: 2019-05-24 17:37:37
categories:
- Npm
tags:
comments: false
---

### 前言
项目中基本一定会使用到npm，以前的项目里还经常使用到cnpm（现在实习公司有自己的源，所以没有用cnpm了），又接触了nrm，今天就来揭开它的面纱！

<!-- more -->

### npm
nodejs的包管理器，用于node包管理（比如安装、卸载、管理依赖等，我们最常就是npm install）



### cnpm

npm安装包是从国外服务器下载，受网络影响大，所以经常咱实行npm install时，又有很多包，就会很慢很慢，还可能出现异常。淘宝团队整了一个npmjs.org镜像，同步频率10分钟一次，保证与官方服务同步，所以咱们使用cnpm速度更快！（界面：下载速度条 我也觉得更好看）



### nrm

管理npm源的工具，比如npm源，cnpm源...通过这个工具，我们可以很方便的切换源，方便自己项目使用

- 查看源
    - nrm ls
        - npm ---- https://registry.npmjs.org/
        - cnpm --- http://r.cnpmjs.org/
- 使用指定源
    - nrm use [name]
- 跳转到源的官网
    - nrm home [name]



### nvm

容易和nrm混淆，nvm是管理node版本的工具。之前项目里npm i失败，提示版本太高，为了降版本使用到的工具。

在安装它的时候，需要把node给卸载了

- 安装nvm
    - brew install nvm
    - mkdir ~/.nvm;
    - open ~/.zshrc
    - 复制export NVM_DIR="$HOME/.nvm" . "/usr/local/opt/nvm/nvm.sh" 到文件中，
    - source ~/.zshrc
- 下载想要的node版本 
    - nvm install [版本号]
- 下载最新的node版本和与之对应的npm版本
    - nvm install latest 
- 使用指定版本号的node
    - nvm use [版本号]
- 查看有多少个版本号的node
    - nvm list
- 卸载
    - nvm uninstall [版本号]    