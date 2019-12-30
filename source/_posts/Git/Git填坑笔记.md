---
title:  Git填坑笔记
date: 2019-07-03 15:58:37
categories:
- Git
tags:
- 填坑
comments: false
---

#### 缺少权限
```shell
git push ![remote rejected] 分支名->分支名 (pre-receive hook declined) 
```

#### 强制Push失败，提示分支被保护
```shell
you are not allowed to push code to protected branches on this project
```

需要工程的创建人在git上把用户添加到被保护的分支的Developer中去。

在工程页面，选择设置，下拉选择Protected Branches -> Developer

