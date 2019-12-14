---
title: git
date: 2019-04-09 15:32:37
categories:
- Git
tags:
comments: false
---

### fetch和merge和pull的区别

 pull相当于git fetch 和 git merge，即更新远程仓库的代码到本地仓库，然后将内容合并到当前分支。

- git fetch：相当于是从远程获取最新版本到本地，不会自动merge

- git merge :  将内容合并到当前分支

- git pull：相当于是从远程获取最新版本并merge到本地



### git merge和git rebase的区别

都是从一个分支获取并且合并到当前分支。



比如在一个feature分支进行新特性的开发，与此同时，master 分支的也有新的提交。

![img](http://s3.mogucdn.com/mlcdn/c45406/190614_7la7ll76bjkk1k45h0cee6512dl9b_1000x805.png)

此时在feature上git 自动会产生一个新的commit(merge commit)



![img](http://s3.mogucdn.com/mlcdn/c45406/190614_2fiehd76ce7ed9j5f0a3g48a2l5jd_1000x877.png)

marge 特点：自动创建一个新的commit

如果合并的时候遇到冲突，仅需要修改后重新commit

优点：记录了真实的commit情况，包括每个分支的详情

缺点：因为每次merge会自动产生一个merge commit，所以在使用一些git 的GUI tools，特别是commit比较频繁时，看到分支很杂乱。



rebase 

本质是变基

![img](http://s3.mogucdn.com/mlcdn/c45406/190614_3hkjle3fd2kbab5a49e44ij7c8la8_1000x774.png)

rebase 特点：会合并之前的commit历史

优点：得到更简洁的项目历史，去掉了merge commit

缺点：如果合并出现代码问题不容易定位，因为re-write了history

合并时如果出现冲突需要按照如下步骤解决

- 修改冲突部分
- git add
- git rebase --continue
- （如果第三步无效可以执行 git rebase --skip）





### git commit 撤销

git add . //添加所有文件

git commit -m "本功能全部完成"

 

执行完commit后，想撤回commit，怎么办？

 

这样凉拌：

**git reset --soft HEAD^**



HEAD^的意思是上一个版本，也可以写成HEAD~1

如果你进行了2次commit，想都撤回，可以使用HEAD~2



**--mixed** 

意思是：不删除工作空间改动代码，撤销commit，并且撤销git add . 操作

这个为默认参数,git reset --mixed HEAD^ 和 git reset HEAD^ 效果是一样的。

 

**--soft**  

不删除工作空间改动代码，撤销commit，不撤销git add . 

 

**--hard**

删除工作空间改动代码，撤销commit，撤销git add . 

注意完成这个操作后，就恢复到了上一次的commit状态。

 

 

**顺便说一下，如果commit注释写错了，只是想改一下注释，只需要：**

git commit --amend

此时会进入默认vim编辑器，修改注释完毕后保存就好了。



[**reset revert**](https://blog.csdn.net/yxlshk/article/details/79944535)

reset 回退到目标版本后，之前的版本没有了，head指向目标版本。需要强制push -f

revert 会反做一个和目标版本内容一致的版本，head指向新的版本 revert -n 版本号 可能要处理冲突



### 为啥要有暂存区

方便将需要添加的文件才添加，如果没有暂存区，就是全部提交，如果一些文件的修改不需要提交，就得一个个修改后，再提交。



### 放弃本地更改
	git checkout .