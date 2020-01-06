---
title:  Linux与Shell
date: 2019-07-19 15:10:37
categories:
- 操作系统
tags:
- Linux
comments: false
---

## 前言
公司实习期间，使用的是分配的mac,之前使用包括自己的电脑都是window，所以现在有机会可以放开胸怀使用Linux命令啦！

首先需要安装[iTerm2](https://www.jianshu.com/p/9c3439cc3bdb)，可以打造舒适的终端体验。有了颜色和背景色，敲起代码来倍爽！

<!-- more -->

## 掌握一些常用的命令：

- 查看目录
    - ls
        - ls -l (ll) 可以看到文件的具体信息：大小、创建时间、可读可写可执行等
        - ls -a  查看所有文件（包括不能看的）
- 跳转目录
    - ~/ 主目录
    - ./ 当前目录
    - ../上一级目录
    - cd `具体路径`
        - 可安装autojump工具，只需要打 j `cd过的文件名` 即可直接跳到想去的地方
- 清屏
    - clear
- 退出命令
    - ctrl+c
- 创建目录
    - mkdir
- 创建文件
    - touch
    - vi(进入vi编辑器)
        - i 进入编辑 
        - 按ESC退出编辑
        - :q 不保存，退出
        - :q! 不保存，强制退出
        - :wq 保存，退出
        - :wq! 保存，强制退出
- 有时安装包后，会要求给~/.zshrc文件中加路径才能生效，需要（可以用上面的vim方法）
    - open ~/.zshrc
    - 在文件中加完后，保存
    - source ~/.zshrc
- 本地跑项目，发现端口被占用
    - lsof -i :端口号 （查看谁在占用）
    - kill 9 端口号 （杀杀杀）
- 查看当前文件的绝对路劲
    - pwd
- 删除
    - rm 文件名
    - rm -rf 文件夹名



## shell命令

突然意识到linux命令和shell命令原来不能统称一体，linux和windows都为操作系统，其中shell是一个脚本，我们可以通过使用它操作系统，但在不同的系统中，使用的shell不同，比如linux默认都是GUNbash shell，而windows默认是cmd。