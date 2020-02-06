---
title: Mac 使用手册
date: 2019-10-29 18:13:47
categories:
- 操作系统
tags:
- Mac
comments: false
---



## 前言

记录 Mac 使用的一些快捷手段+开发工具

<!-- more -->



## mac 快捷键

| 功能           | 快捷键            |
| -------------- | ----------------- |
| 最小化当前应用 | cmd+m             |
| 最小化所有应用 | cmd+f3            |
| 关掉当前页面   | cmd+w             |
| 新增页面       | cmd+t             |
| 复制/粘贴      | cmd+c cmd+v cmd+x |
| 开发者工具     | cmd+option+i      |
| 全屏/退出全屏  | cmd+control+f     |



## 装系统
- iterm2
- [oh-my-zsh](https://www.jianshu.com/p/9c3439cc3bdb)（iterm2主题，高亮等，更美观）
- vscode
  - [vscode 插件](https://blog.csdn.net/shenxianhui1995/article/details/81604818)
  - [eslint + prettier](https://wiki.n.miui.com/pages/viewpage.action?pageId=177314186)
    - 定义了变量并且使用了，但是eslint还是报未使用的错，可以在声明变量的当前行加上一条注释 `// eslint-disable-line no-unused-vars`
  - [Ts-node](https://cloud.tencent.com/developer/article/1499075)：实现ts自动编译，又没有产生js文件
- git
- nvm
- node
- nginx
- vpn
- postman
- switchHosts
- charles
- office

> 下载homebrew，可以使用 brew install iterm2/git/nvm/nginx，其中下载了nvm，默认下载node，npm，nvm版本根据项目规定

- [配置ssh](https://www.jianshu.com/p/94b39f278214)

  

## 修改图片像素

可以直接在图片预览中改像素大小



## 实现code . 打开vscode

运行 VS code并打开命令面板（ ⇧⌘P ），然后输入 shell command 找到: Install ‘code' command in PATH ，点击安装就行了。

在终端中输入命令

```shell
$ code .
```

即可在 vs code 中打开当前文件夹。