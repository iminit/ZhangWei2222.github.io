---
title: hexo搭建个人博客
date: 2017-08-21 15:15:47
categories:
- 前端技术篇
tags:
- hexo
- 个人博客
comments: false
---

### 1. 安装Git Bash
我一直不太喜欢在cmd中操作各种命令，所以挑了这个比较好使的Git Bash, 我的是windows环境，所以下载windows版本并安装就可以了。

- 下载地址https://git-for-windows.github.io/
- 安装步骤：双击下载好的exe文件，一路next就好啦
- 安装好后，打开gitbash，查看版本：
    - 命令：git version 
- 然后你就可以在这里发挥你的聪明才智了

### 2. 安装NodeJs
Hexo是基于nodeJS环境的静态博客，里面的npm工具很有用啊，所以还是老老实实把这玩意儿装了吧

- 下载地址https://nodejs.org/en/(说明：LTS为长期支持版，Current为当前最新版)
- 安装步骤：反正下载好msi文件后，双击打开安装，也是一路next，不过在Custom Setup这一步记得选 Add to PATH ,这样你就不用自己去配置电脑上环境变量了，装完在按 win + r 快捷键调出运行，然后输入cmd确定，在cmd中输入path可以看到你的node是否配置在里面（环境变量），没有的话你就自由发挥吧。
- 查看版本：
    - 命令：node -v
- 又到自由发挥的时候了

### 3. 安装hexo
看到这么多安装，千万不要紧张，小哥哥小姐姐们一定要稳住，别怕，因为后面的东西都是在gitbash中用npm工具安装就好了。

- 先创建一个文件夹（用来存放所有blog的东西），然后cd到该文件夹下。
- 安装hexo命令：npm i -g hexo
- 安装完成后，查看版本：hexo -v
- 初始化命令：hexo init ，初始化完成之后打开所在的文件夹可以看到以下文件：
- 解释一下：
    - node_modules：是依赖包
    - public：存放的是生成的页面
    - scaffolds：命令生成文章等的模板
    - source：用命令创建的各种文章
    - themes：主题
    - _config.yml：整个博客的配置
    - db.json：source解析所得到的
    - package.json：项目所需模块项目的配置信息
- 做好这些前置工作之后接下来的就是各种配配配置了。

#### 4. 搭桥到github
- 没账号的创建账号，有账号的看下面。
    - 创建一个repo，名称为yourname.github.io, 其中yourname是你的github名称，按照这个规则创建才有用哦，如下：
- 回到gitbash中，配置github账户信息（YourName和YourEail都替换成你自己的）：
    - git config --global user.name 'yourname'
    - git config --global user.email 'youremail'
- 创建SSH
    - 在gitbash中输入：ssh-keygen -t rsa -C “youremail@example.com，生成ssh。然后按下图的方式找到id_rsa.pub文件的内容。
- 将上面获取的ssh放到github中：
- 添加一个 New SSH key ，title随便取，key就填刚刚那一段。
- 在gitbash中验证是否添加成功：ssh -T git@github.com
- 完成下一步你就成功啦！

#### 5. 一步之遥
- 用编辑器打开你的blog项目，修改_config.yml文件的一些配置(冒号之后都是有一个半角空格的)：
    
```
deploy:
    type: git
    repo: https://github.com/YourgithubName/YourgithubName.github.io.git
    branch: master
```
- 回到gitbash中，进入你的blog目录，分别执行以下命令：
    - hexo clean
    - hexo generate
    - hexo server

注：hexo 3.0把服务器独立成个别模块，需要单独安装：npm i hexo-server。

- 打开浏览器输入：http://localhost:4000
- 接着你就可以遇见天使的微笑了~

#### 6. 上传到github
- 先安装一波：npm install hexo-deployer-git -–save（这样才能将你写好的文章部署到github服务器上并让别人浏览到）
- 执行命令(建议每次都按照如下步骤部署)：
```
hexo clean
hexo generate
hexo deploy
```
- 注意deploy的过程中要输入你的username及passward。如下：
- 在浏览器中输入http://yourgithubname.github.io就可以看到你的个人博客啦，是不是很兴奋！
- 感觉gitbash中东西太多的时候输入clear命令清空。

#### 7. 绑定个人域名
- 登录进入万网的域名控制台，点击”域名和网站”中的”云DNS”
- 点击对应域名的”解析”
- 点击添加解析，记录类型选A或CNAME，A记录的记录值就是ip地址，github(官方文档)提供了两个IP地址，192.30.252.153和192.30.252.154，这两个IP地址为github的服务器地址，两个都要填上，解析记录设置两个www和@，线路就默认就行了，CNAME记录值填你的github博客网址。如我的是whitescholars.github.io。
- 这些全部设置完成后，此时你并不能要申请的域名访问你的博客。
    - 接着你需要做的是在hexo根目录的source文件夹里创建CNAME文件，不带任何后缀，里面添加你的域名信息，如：penglei.com。
- 运行hexo指令使配置修改起效：
    - hexo g  //会在public中生成一个GNAME文件
    - hexo d  //修改内容提交到github博客上
- 域名绑定：
- 这是比较重要的一步，打开博客在Github中的地址，然后切换到 Settings页，设置Custom domain内容为我们自己的域名值（例如：我的域名是linshuhe.tech），点击Save按钮保存

实践证明如果此时你填写的是www.penglei.top那么以后你只能用www.penglei.top访问，而如果你填写的是penglei.top。那么用www.penglei.top和penglei.top访问都是可以的。重新清理hexo,并发布即可用新的域名访问。

##### 搭建完成访问出现404 可能的原因是：

- 绑定了个人域名，但是域名解析错误。
- 域名解析正确但你的域名是通过国内注册商注册的，你的域名因没有实名制而无法访问。
- 你认为配置没有问题，那么可能只是你的浏览器在捣鬼，可尝试清除浏览器缓存再访问或者换个浏览器访问。
- 也有可能是你的路由器缓存导致的错觉，所以也可以尝试换个局域网访问你的网站。
- 最有可能的原因是你下载的hexo有问题，导致所有的东西都上传到了github,而导致index页面在主域名的下一级目录。你可以尝试查看上传的内容，找到index页面，在域名后面添加下一级目录。若能访问index页面（此时样式可能是乱的），则证明是hexo安装有问题，笔者当时遇到的就是这个问题。可卸载重新安装。
- 注：1，2默认你的CNAME文件配置没有问题，如果没有绑定个人域名，则不需要CNAME文件。

#### 8.修改主题
- 下载主题到theme
- 站点配置文件中(根目录下的_config.yml)，把theme改成对应的名字
- hexo clean
- hexo g && hexo d

##### 如何配置标签？

```
title: 标签测试文章
tags:
  - Testing
  - Another Tag
```
