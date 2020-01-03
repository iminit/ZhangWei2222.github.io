---
title:  ES6-export、export default区别
date: 2019-05-28 09:12:37
categories:
- Es6
comments: false
---

## 前言

在项目中发现有的引用是这样的

```javascript
import { test1 } form 'foo1.js'
import test2 form 'foo2.js'
```

怎么有的有花括号，有的没有呢？？？

<!-- more -->

观察 foo1.js 和 foo2.js，会发现它们方法、变量的导出方式是不一样的


```javascript
foo1.js
export const str = 'hello'

foo2.js
export default function isTest(str) {
    return str
}
```
看来是因为，export和export default的原因



## 区别

在es6中，export 和 export default均可以导出常量、函数、文件、模块等，我们可以在其他的文件或模块中使用 import 导入

1. 在一个文件或模块中，export、import可以有多个，export default仅有一个
1. 通过export方式导出，在导入时要加{}，export default不用
1. export可以直接导出变量表达式，export default不行