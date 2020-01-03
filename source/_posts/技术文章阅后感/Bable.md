---
title: 阅后感-Babel
date: 2019-11-13 18:30:47
categories:
- 技术文章阅后感
comments: false
---



[Babel](https://mp.weixin.qq.com/s/BEhMBXqMbQq8ZBJAqzxyDA)

Babel的实现：将源码转换 AST(抽象语法树)之后，通过便利AST树，对树做一些修改，然后再将AST转成code，即成源码。可以让我们的代码（es6,es7..语法）在浏览器正常显示。

Babel的设计（文章用汽车发动机作比喻，好挺好理解）

- @babel/core AST转换的核心
- @babel/cli 打包工具
- @babel/plugin* Babel 插件机制，Babel基础功能不满足的时候,手动添加些
- @babel/preset-env 预设了很多语法和API的转换插件，不需要在单独的去安装某个已经列入标准的插件。同时，具备了可以智能的根据browserslist选项,导出项目中使用的语法
- @babel/polyfill 把浏览器某些不支持API，兼容性代码全部导入到项目,不管你是不是用到,缺点是代码体积特别大和污染全局变量
- @babel/runtime和@babel/plugin-transform-runtime 把你使用到的浏览器某些不支持API，按需导入,代码少，不污染全局变量

其中 presets 加载顺序和一般理解不一样，是倒序的；plugins 正序进行编译；plugins 优先于 presets进行编译

Babel 把 Javascript 语法 分为 syntax （箭头函数，let,const,class, 依赖注入 Decorators,等等这些）和 api（可以通过 函数重新覆盖的语法 ，类似 includes,map,includes,Promise）

Babel 只是转换 syntax 层语法,所有需要 @babel/polyfill 来处理API兼容,又因为 polyfill 体积太大，所以通过 preset的 useBuiltIns 来实现按需加载,再接着为了满足 npm 组件开发的需要 出现了 @babel/runtime 来做隔离