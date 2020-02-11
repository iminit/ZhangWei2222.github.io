---
title:  Npm audit fix 报错
date: 2020-02-11 18:12:37
categories:
- Npm
tags:
comments: false
---


在 `npm install` 的时候，发现有这样的提示

```shell
found 13 vulnerabilities (1 low, 8 moderate, 4 high)
  run `npm audit fix` to fix them, or `npm audit` for details
```

<!-- more -->

如果忽视它，是跑不起项目的。必须正式这个报错，所以按照提示`npm audit fix`，执行后，又会报错

```shell
 (use `npm audit fix --force` to install breaking changes; or refer to `npm audit` for steps to fix these manually)
```

这时，**千万不能`--force`**，因为他会强制安装最新的依赖项，有可能把自己项目的依赖搞乱了。老老实`npm audit`发现错误吧，按照实际情况，不去纠正错误也是可以运行的，按需？

```shell
# 扫描项目漏洞把不安全的依赖项自动更新到兼容性版本
npm audit fix

# 强制执行 audit fix 安装最新的依赖项（toplevel）
npm audit fix --force
```

