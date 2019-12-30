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



#### 2. 强制Push失败，提示分支被保护

```shell
you are not allowed to push code to protected branches on this project
```

需要工程的创建人在 git 上把用户添加到被保护的分支的 Developer 中去。

在工程页面，选择设置，下拉选择 `Protected Branches -> Developer`



#### 3. 装oh-my-zsh时总是报错

```shell
localhost% sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)" 
curl: (7) Failed to connect to raw.githubusercontent.com port 443: Operation timed out
```

解决：配置git账号

```shell
git config --global user.name 'xx'
git config --global user.email 'xx@xxx.com'
git config -l
```



