---
title: git commit 报 Changes not staged for commit
date: 2020-04-30 18:34:37
categories:
- Git
comments: false
---

问题描述：提交时发现有几个文件没有提交成功，再次 `git add . git commit ` 发现报错：`Changes not staged for commit`

原因：`git add .`路径出错。。。之前在子目录打包，忘记跳回根目录了

