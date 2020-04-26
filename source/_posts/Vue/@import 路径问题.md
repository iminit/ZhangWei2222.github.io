---
title: vue-@import 路径问题
date: 2020-04-26 17:18:47
categories:
- Vue
comments: false
---

想要采用别名将公共样式文件的路径引入，应当如此：`@import url("~@/common/style/Variable.less");`

因为 CSS loader 会把非根路径的 url 解释为相对路径， 加 **~ 前缀** 才会解释成模块路径

