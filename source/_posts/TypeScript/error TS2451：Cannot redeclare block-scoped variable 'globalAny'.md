---
title: error TS2451：Cannot redeclare block-scoped variable 'globalAny'
date: 2020-04-28 18:14:47
categories:
- TypeScript
comments: false
---

**报错原因：**在默认状态下，`typescript` 将 `DOM typings` 作为全局的运行环境，所以当我们声明 `globalAny` 时， 与 `DOM` 中的全局 `window` 对象下的 `globalAny` 属性出现了重名。

<!-- more -->

**解决办法：** 将脚本封装到模块内，`module` 有自己的作用域，自然不会与全局作用域的变量产生冲突。

> 在 Typescript 中，只要文件存在 import 或 export 关键字，都被视为 module

在脚本最后一行，添加 `export {};`。将文件声明为 `module`， 变量  `globalAny`  被限制在了 `module` 的作用域下，因此不会与全局的 `globalAny`  产生冲突。