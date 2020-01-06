---
title: Git 重命名文件
date: 2019-12-05 18:19:37
categories:
- Git
tags:
comments: false
---



## 前言

看了胜凯发的关于git 文件重命名的文章，自己尝试了一下

首先：直接更改文件名，git status ，追踪不到更改

<!-- more -->



## 方法一

```shell
$ git mv src/components/rename.vue src/components/Rename.vue

$ git status                                                
# 位于分支 master
# 您的分支与上游分支 'origin/master' 一致。
# 
# 要提交的变更：
#   （使用 "git restore --staged <文件>..." 以取消暂存）
#         重命名：   src/components/rename.vue -> src/components/Rename.vue

$ git commit -m '把rename改成Rename'
# [master c05b49d] 把rename改成Rename
#  1 file changed, 0 insertions(+), 0 deletions(-)
#  rename src/components/{rename.vue => Rename.vue} (100%)  // 注意这个地方

$ git push
$ git pull 刷新一下就出来新命名的了
```

在 git 中使用 `mv` 命令的时候，会自动 add 所以不用额外敲 git add .



## 方法二

```shell
$ git config core.ignorecase false 

# 改文件名

$ git status
# 位于分支 master
# 您的分支与上游分支 'origin/master' 一致。
# 
# 未跟踪的文件:
#   （使用 "git add <文件>..." 以包含要提交的内容）
#         src/components/Rename.vue
# 
# 提交为空，但是存在尚未跟踪的文件（使用 "git add" 建立跟踪）

git add . 
git commit -m '把rename改成Rename 测试方法2 全局设计  git config core.ignorecase false'
# [master 23280e5] 把rename改成Rename 测试方法2 全局设计  git config core.ignorecase false
#  1 file changed, 18 insertions(+)
#  create mode 100644 src/components/Rename.vue // 注意这个地方
```

可以看出它是新增了一个文件，看一些文章显示不同的系统可能会出现两个文件，不推荐