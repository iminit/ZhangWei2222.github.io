---
title: 配置next主题
date: 2019.01.24 15:15:47
categories:
- 个人博客
tags:
- Hexo
- Next
comments: false
---

# 安装Next

<!-- more -->

### 下载主题
在博客根目录

```
git clone https://github.com/iissnan/hexo-theme-next themes/next
```

### 启用主题

修改站点配置文件_config.yml


```
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next
```

### 验证主题

启用主题后，先清除缓存


```
hexo clean
```


启用Hexo本地站点，验证主题


```
hexo s --debug
```

## 主题设定

### 选择皮肤

- Muse - 默认 Scheme，这是 NexT 最初的版本，黑白主调，大量留白
- Mist - Muse 的紧凑版本，整洁有序的单栏外观
- Pisces - 双栏 Scheme，小家碧玉似的清新

可以在主题配置文件进行切换，把注释#去除即可


```
#scheme: Muse
#scheme: Mist
scheme: Pisces
```

### 设置语言

在站点配置文件，设置language

```
language: zh-Hans
```

### 设置菜单

菜单配置包括三个部分，第一是菜单项（名称和链接），第二是菜单项的显示文本，第三是菜单项对应的图标。 NexT 使用的是 Font Awesome 提供的图标， [Font Awesome](https://fontawesome.com/?from=io) 提供了 600+ 的图标，可以满足绝大的多数的场景，同时无须担心在 Retina 屏幕下 图标模糊的问题。

主题配置文件

**设定菜单内容**

```
menu:
  home: /
  archives: /archives
  #about: /about
  #categories: /categories
  tags: /tags
  #commonweal: /404.html
```
> 若你的站点运行在子目录中，请将链接前缀的 / 去掉

NexT 默认的菜单项有（标注  ！的项表示需要手动创建这个页面）：

键值 | 设定值 | 显示文本（简体中文）
---|--- |---
home | home: / | 主页
archives | archives: /archives | 归档页
categories | categories: /categories | 分类页！
tags | tags: /tags | 标签页！
about | about: /about | 关于页面！ 
commonweal | commonweal: /404.html | 公益 404！

**设置菜单项的显示文本**

在第一步中设置的菜单的名称并不直接用于界面上的展示。Hexo 在生成的时候将使用 这个名称查找对应的语言翻译，并提取显示文本。这些翻译文本放置在 NexT 主题目录下的 languages/{language}.yml （{language} 为你所使用的语言）。

以简体中文为例，若你需要添加一个菜单项，比如 something。那么就需要修改简体中文对应的翻译文件 languages/zh-Hans.yml，在 menu 字段下添加一项：


```
menu:
  home: 首页
  archives: 归档
  categories: 分类
  tags: 标签
  about: 关于
  search: 搜索
  commonweal: 公益404
  something: 有料
```

**设定菜单项的图标**


```
menu_icons:
  enable: true
  # Icon Mapping.
  home: home
  about: user
  categories: th
  tags: tags
  archives: archive
  commonweal: heartbeat
```

### 设置头像
编辑 主题配置文件，修改字段 avatar


```
放置在 source/images/ 目录下 
配置为：avatar: /images/avatar.png
```

### 设置作者昵称
编辑 站点配置文件，设置 author 为你的昵称。

### 站点描述
编辑 站点配置文件，设置 description 字段为你的站点描述。站点描述可以是你喜欢的一句签名:)

## 百度统计

- 登录[百度统计](https://tongji.baidu.com/web/27265037/welcome/login)，添加网站后，到代码获取页面
- 复制hm.js后面那串统计脚本id
- 编辑 主题配置文件， 修改字段 baidu_analytics 字段，值设置成你的百度统计脚本 id。


### 创建 分类

#### 生成分类页

```shell
$ hexo new page categories
```

成功后进入文件，打开后默认

```shell
---
title: 文章分类
date: 2017-05-27 13:47:40
---
```

添加type

```shell
---
title: 文章分类
date: 2017-05-27 13:47:40
type: "categories"
---
```

保存 关闭

### 创建 标签

#### 生成标签页

```shell
$ hexo new page tags
```

成功后进入文件，打开后默认

```shell
---
title: 标签
date: 2017-05-27 13:47:40
---
```

添加type

```shell
---
title: 分类
date: 2017-05-27 13:47:40
type: "tags"
---
```

保存 关闭

### 创建 新文章

```shell
hexo new '文章名'
```

打开 scaffolds/post.md


```shell
---
title: jQuery对表单的操作及更多应用
date: 2017-05-26 12:12:57
categories: 
- web前端
tags:
- jQuery
- 表格
- 表单验证
---
```

注意categories只有一个，tags允许多个

### 侧边栏社交链接

主题配置文件


```
social:
  GitHub: https://github.com/your-user-name
  Twitter: https://twitter.com/your-user-name
  微博: http://weibo.com/your-user-name
  豆瓣: http://douban.com/people/your-user-name
  知乎: http://www.zhihu.com/people/your-user-name
  # 等等
  
# Social Icons
social_icons:
  enable: true
  # Icon Mappings
  GitHub: github
  Twitter: twitter
  微博: weibo
```
注意要去掉social,social icons的注释！！以及enable要设置为true

### 腾讯公益404页面

使用方法，新建 404.html 页面，放到主题的 source 目录下，内容如下：


```html
<!DOCTYPE HTML>
<html>
  <head>
    <meta http-equiv="content-type" content="text/html;charset=utf-8;"/>
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
    <meta name="robots" content="all" />
    <meta name="robots" content="index,follow"/>
    <link rel="stylesheet" type="text/css" href="https://qzone.qq.com/gy/404/style/404style.css">
  </head>
  <body>
    <script type="text/plain" src="http://www.qq.com/404/search_children.js"
            charset="utf-8" homePageUrl="/"
            homePageName="回到我的主页">
    </script>
    <script src="https://qzone.qq.com/gy/404/data.js" charset="utf-8"></script>
    <script src="https://qzone.qq.com/gy/404/page.js" charset="utf-8"></script>
  </body>
</html>
```

### 站点建立时间

这个时间将在站点的底部显示，例如 © 2017 - 2019。 编辑 主题配置文件，字段 since。

```
footer:
  since: 2013
```

### 设置「背景动画」
NexT 自带两种背景动画效果

编辑 主题配置文件， 搜索 canvas_nest 或 three_waves，根据您的需求设置值为 true 或者 false 即可：

注意： three_waves 在版本 5.1.1 中引入。只能同时开启一种背景动画效果。

