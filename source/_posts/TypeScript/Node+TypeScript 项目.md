---
title: Node+TypeScript 项目
date: 2019-11-20 18:35:47
categories:
- TypeScript
comments: false
---



在 Node+TypeScript 项目中，自己新建了一个分支，尝试把js文件改成ts文件。发现了几个有趣的地方：

1. ts文件使用`import`引入外部资源，js文件使用`require`：`node.js module`是`commonjs` ，`ts` 一般推荐使用`es6` 规范
2. `Argument of type 'any' is not assignable to parameter of type 'never'.`一般出现在数组报错，需要给数组定义类型

实操后还是能亲身感受到`typescript`的便利性：可编译时检测，定义类型，减少出错的几率，提高开发的效率