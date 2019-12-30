---
title:  git区、状态、修改
date: 2019-07-31 09:17:37
categories:
- Git
tags:
comments: false
---

## 四个工作区
#### 工作区
我们平时存放项目代码的地方，但是是指受Git控制的，被Git跟踪记录的文件

#### 暂存区
类似缓存区域，临时保存文件

#### 本地仓库
连接本地代码和远程代码的枢纽，不能联网时本地代码可以先提到本地仓库；或叫版本（.git），HEAD指向最新放入仓库的版本

#### 远程仓库
托管代码的服务器

## 状态
#### 未跟踪
未跟踪的文件改动不受到Git的版本控制

在工作区新加入一个文件时，是处于未跟踪状态的，需要add 将其加入跟踪，并同时放在暂存区

初次克隆某个仓库时，工作区中的所有文件都是已跟踪且未修改的状态

#### 已跟踪
被纳入版本控制的文件，又分
1. 未修改（unmodified）
1. 已修改（modified）
1. 已暂存（staged）
1. 已提交(Committed)
1. 已推送(Pushed)

## 代码提交和同步流程
![image](https://s11.mogucdn.com/mlcdn/c45406/190731_4j8i0gi2ijj9bclbja540896lae54_798x388.png)
1. 文件增删改，变为 已修改状态
1. git add，变为 已暂存状态
    1. git add . 暂存所有更改的文件
    1. git add [指定的文件]
1. git commit，变为 已提交状态
    1. git commit -m '[描述]'
    1. git commit -m \` [描述]\n[描述]\n[描述]\`
    2. git commit -am 不需要写add,会直接把已跟踪的文件一起推到暂存区（注意：不适用于未被git跟踪的）
1. git push，变为已推送状态
    1. git push -u origin 本地分支 远程分支 第一次要关联上
    1. git push 之后直接push
    1. git branch 查看本地仓库分支
    1. git branch -a  查看本地仓库和本地远程仓库(远程仓库的本地镜像)的所有分支

## 五种状态间的撤销更改
![image](https://s11.mogucdn.com/mlcdn/c45406/190731_37a9def9c890ceca88kh1d7501fd3_787x412.png)
#### 已修改，但未暂存（未git add）

```shell
git diff 列出所有修改
git diff [指定文件的修改]
git checkout . 撤销工作区的所有修改
git checkout [撤销工作区指定文件的修改]
git clean -f 撤销新增的未跟踪文件
git clean -df 撤销新增的未跟踪文件和文件夹
```

#### 已暂存，未提交（git add了，未git commit）

```shell
git diff --cached 查看暂存区和本地仓库的差异
git reset (--soft) 回退到已修改状态，内容还在工作区
git reset --hard 回退到未修改状态，清空所有的修改
```

#### 已提交，未推送（git commit了，未git push）
commit后，仓库中会生成版本号(hash值)，标志这次的提交。之后可以按照版本会进行控制

```shell
git diff [分支1] [分支2] 查看两个分支的差异
git diff 本地仓库分支名 origin/远程仓库分支名 查看本地仓库和本地远程仓库的差异
git reset (--soft) 回退到已修改状态，修改还在工作区
git reset --hard HEAD^ 回退到本地仓库上一个版本（可以说是修改全没了）
git reset --hard [hash值] 回退到任意版本
git reset --hard origin/远程仓库分支名 回退到与本地远程仓库一致
```

#### 已推送到远程

```shell
git push -f origin 远程仓库分支名（慎用） 强制覆盖远程分支
git push -f 如果关联过，可以忽略分支名（慎用）
```

## 其他

#### 删除

```shell
rm 物理上的删除，版本库还未删除
git rm 工作区和版本库都删除了
git rm --cached 工作区还在，版本库删除了。适用于不想被版本控制的文件
```


#### 移动

```shell
git mv test.txt TEST 把test.txt移到test

实际上执行
mv test.txt TEST
git rm test.txt 
git add TEST
```


#### 查看版本号

```shell
git log 查看提交过的版本信息，会有很多信息：作者，时间等。可以加--pretty=oneline，只会显示版本号和commit的内容

git reflog 查看所有的提交记录，包括已经删掉的和reset的操作
例如执行 git reset --hard HEAD~1，退回到上一个版本，用git log则是看不出来被删除的commitid，用git reflog则可以看到被删除的commitid，我们就可以买后悔药，恢复到被删除的那个版本
```

#### 恢复版本
我们恢复版本一般用reset和revert

reset作用：改变HEAD的位置，可以改成之前存在的某个版本。适用于，想恢复到某个版本，那之后的版本不要了的场景。

revert作用：反做。适用于，想撤销某次版本的提交，但不影响后面的版本（revert后会新建一个版本）。比如功能分支合并到主分支，功能分支发现有问题，又不能影响主分支后面的代码，就可以把它revert掉。

```shell
git revert -n 版本号
```


## 易混淆
- git reset 可以作用在暂存区（git add了），可以回退到工作区；也可以作用在本地仓库（git commit了），可以回退/前进到任意版本
- git checkout 可以撤销工作区的修改