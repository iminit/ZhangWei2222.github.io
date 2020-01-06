---
title:  Git-rebase
date: 2019-08-02 23:02:37
categories:
- Git
tags:
comments: false
---

## 前言
闻rebase色变，最近刚好在项目中用到了rebase合并分支与commit号，特此记录（非常艰难的探索）

<!-- more -->

## 合并分支

### 问题

最开始同事和我约定，在自己的分支中，先git pull rebase origin master，再切到master分支，merge 自己的分支，最后push master分支。往后继续在自己的分支上开发，如有修改再重复上面的步骤。

我一看rebase（确实对它有点恐慌），又想之前咱们合并功能分支不是直接在master分支上merge？怎么突然又rebase 又merge，跳来跳去的呢？

和同事讨论了一会，为啥在自己的分支上要rebase而不是merge？当时两个人都觉得merge是ok的。包括后来我自己在测试分支上尝试，也没看出个所以然。

### 实践
最后两个人对着电脑又使用测试分支尝试了一波

#### 主分支merge功能分支
我们平时经常是 主分支 **merge** 功能分支 ，这样做的结果是，**功能分支的commit号按时间顺序插到主分支的commit号里面，最后有一个merge commit 号**。假如咱们还想继续在功能分支开发，就需要和主分支保持联系，从与自己的远程分支联系变成和主分支联系，这显示是不科学的，成本很高。每次都要从主分支pull代码，解决冲突，主分支又再merge，主分支上的log将惨不忍睹，何况不止一个功能分支

主分支Merge功能分支，适用于之后不再在功能分支上开发的情况。


#### 功能分支merge 主分支rebase/merge
如果我们想后面可以继续在功能分支上开发，显然需要避免需要多次解决冲突的情况

先在功能分支 merge 主分支，主分支的commit号按时间顺序插到功能分支上，最后形成merge commit号。随后在主分支rebase的话，最后是需要强制push的，肯定不能再主分支上强制push **（主分支rebase不合理）**；如果是merge的话.....那就没啥必要功能分支再merge了，和上面情况一致 **（主分支merge不合理）**

#### 功能分支rebase 主分支rebase/merge

在 功能分支 上 rebase 主分支，会以主分支为主，如果功能分支有与主分支不同的commit号，会一次次合并，不管有没有冲突，产生的都是新的commit 号。（注意，如果有冲突，解决完需要 git add .再git rebase --continue，最后需要强制push -f），这个时候功能分支就能与主分支保持最新的代码 **（功能分支rebase合理）**；、

之后主分支如果rebase功能分支，就需要再次一个个commit号合并 **（主分支rebase 不合理）**，使用merge，就只需要解决一次冲突，产生一个merge commit号 **(主分支merge合理)**

### 总结
折腾了半天，根据上面各种情况的推算，最后还是维持之前的约定，往后开发 合并功能分支 步骤如下：

- 功能分支
    - git pull --rebase origin master
    - git add .
    - git rebase --continue
    - git push -f
- 主分支
    - git merge 功能分支
    - git push



## 合并commit号

### 问题
在项目发开中思考了一个问题，因为改版模块，其实改动不大，都是一个个小的功能，我习惯于把一个功能写好，就push到分支上。最后会发现很多commit，其中有可能针对同一个功能，前后改动很多次，log就很难看了。

### 实践

恰好看到了rebase，自测了一下，可以使用git rebase -i 版本号（或 HEAD~回退的版本数）合并commit。输入命令后，会调出vim编辑器，显示想要操作的commit，按照提示（一般第一个p,后面几个s表合并前面的commit），可以对commit进行保留，删除等等。最后需要强制push -f，覆盖掉远端仓库的版本，不然又来回来了。

注意，可能几次提交的版本都对某个地方进行修改，需要手动合并（这个代价还挺大的，所以rebase还是得看情况使用）。而且最后需要强制提交，1是我们可能还需要在项目中设置分支保护 取消，2是其实不太安全。