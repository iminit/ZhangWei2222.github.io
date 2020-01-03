---
title: 在一台电脑使用两个github账号
date: 2019-11-20 18:13:37
categories:
- Git
comments: false
---



本来想着在自己本身的`github`上拉一个项目，发现没有权限，emmmm，确实，这个电脑的`git`账号用的公司的账号，当然`clone`不成功。在同一台电脑上使用多个`github`账号，这个情景以后应该会用的蛮多，所以学习了一下，下面记录：

<!-- more -->



## 步骤

```shell
$ cd ~/.ssh

# 为个人账号生成 SSH Key
$ ssh-keygen -t rsa -C 'personal@maiil.com' -f id_rsa_personal 

# 查看生成的 SSH Key，复制到github仓库，添加SSH
$ cat ~/.ssh/id_rsa_personal.pub

# 测试 SSH Key
$ ssh -T git@github.com

# 把Key加到SSH agent上，因为不是使用默认的.ssh/id_rsa，所以需要告诉 SSH agent新Key的位置
$ sh-add ~/.ssh/id_rsa_personal

# 配置.ssh/config，先ls看看有没有config文件，没有就要touch config
#default github
Host github.com
  HostName github.com
  IdentityFile ~/.ssh/id_rsa
 
Host github_wei
  HostName github.com
  IdentityFile ~/.ssh/id_rsa_personal
```

之后通过使用别名`github_wei`来明确说明使用`id_rsa_personal`的`SSH key`来连接`github`

```shell
# clone到本地
$ git clone git@github_wei:xxxx/test.git

# push到github上去
$ git remote add origin git@github_wei:xxxx/test.git
$ git push origin master
```

也可以通过特定的`repo`下执行下面的命令

```shell
$ git config user.name 'newname'
$ git config user.email 'newemail'

# git config --global --unset user.name 取消全局设置
# git config --global --unset user.email 取消全局设置
```



## 在用个人账号pull,push操作时出现的问题

```shell
$ git push --set-upstream origin master
# Enter passphrase for key '/Users/wei/.ssh/id_rsa_personal': 
# Enter passphrase for key '/Users/wei/.ssh/id_rsa_personal': 
# Enter passphrase for key '/Users/wei/.ssh/id_rsa_personal': 
# To github_wei:ZhangWei2222/analysis-code.git
#  ! [rejected]        master -> master (non-fast-forward)
# error: 推送一些引用到 'git@github_wei:ZhangWei2222/analysis-code.git' 失败
# 提示：更新被拒绝，因为您当前分支的最新提交落后于其对应的远程分支。
# 提示：再次推送前，先与远程变更合并（如 'git pull ...'）。详见
# 提示：'git push --help' 中的 'Note about fast-forwards' 小节。
```

**原因：** 远程仓库和本地不同步引起的

**解决：**

```shell
$ git remote add origin https://github.com/yuanzb/yuanzb.git  
$ git fetch origin    //获取远程更新
$ git merge origin/master //把更新的内容合并到本地分支
```

然后在merge的时候发现

```shell
$ git merge origin/master
$ fatal: 拒绝合并无关的历史
```

**原因：** 本地初始化的项目 与 github 版本不一致, 导致无法提交

**解决：** 在pull 时候, 添加–allow-unrelated-histories参数 即可.

```shell
$ git pull origin master --allow-unrelated-histories  
# 有冲突，解决，再add commit git pull git push…
$ git push --set-upstream origin master
```