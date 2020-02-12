---
title: Vscode-添加 Git 管理
date: 2020-02-08 17:43:47
categories:
- 开发
tags:
- vscode
comments: false
---

发现 Vscode 的 Git管理区一片空白，还提示 “当前没有源代码管理提供程序进行注册 ”，不能看到更改状态和分支，非常不痛快。

最初以为是插件没装好，安装卸载了几次，发现还是那样，百度了一下，需要配置 `git.path`

<!-- more -->

1. `file -> preferences -> settings-> 输入git.path -> 点击"edit in settings.json"`

2. 在json文件中添加以下代码：（后边为自己的git.exe路径）

   `"git.path":"D:/install/Git/bin/git.exe"`

3. 重启 Vscode

