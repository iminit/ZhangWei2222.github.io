---
title: Cherry-pick
date: 2019-11-18 18:27:37
categories:
- Git
comments: false
---



> `git cherry-pick`可以理解为”挑拣”提交，它会获取某一个分支的单笔提交，并作为一个新的提交引入到你当前分支上。 当我们需要在本地合入其他分支的提交时，如果我们不想对整个分支进行合并，而是只想将某一次提交合入到本地当前分支上，那么就要使用`git cherry-pick`了

<!-- more -->

1. 当前功能分支 `git log` 拿到`commitId`
2. `checkout`到主分支
3. `git pull`
4. `git cherry-pick commitId`
5. 如果没有冲突，直接 `git push` & `npm run publish`
6. 有冲突，解决，`git add`& `git cherry-pick --continue`

