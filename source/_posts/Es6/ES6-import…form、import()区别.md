---
title: ES6-import…form、import()区别
date: 2019-05-28 11:47:37
categories:
- Es6
comments: false
---

## 前言

在咱们项目中，发现import会有两种形式写法

```javascript
import Vue form 'vue'
routes: [
    {
        path: '/home',
        component: () => import('./sre/home.vue')
    }
]
```

import…form、import()之间有啥区别呢？让我们揭开它们的面纱，let's go

<!-- more -->

## 区别

这两个都是Es6新增的特性，表示引用加载模块

| import…form                                                  | import()                             |
| ------------------------------------------------------------ | ------------------------------------ |
| 静态加载：先于模块内的其他语句执行。所以import…form命令只能在模块的顶层，不能在代码块之中 | 动态加载：运行时执行，异步加载       |
| Es6-Es5转译方面，只要有babel就行（webapce 配置 babel-loader） | Es6-Es5转译方面，需要额外的babel配置 |



## 注意

1、import() 函数加载模块成功后，这个模块会作为一个对象，当做then方法的参数(简单地说，就是会返回一个Promise对象)。因此，可以使用 **对象解构赋值** 的语法，获取输出接口

```javascript
import('./test.js').then( ({export1, export2}) => {   
    //...
})
```

2、import() 函数可以用在async函数中

```javascript
async function main() {
  const myModule = await import('./myModule.js');
  const {export1, export2} = await import('./myModule.js');
  const [module1, module2, module3] =
    await Promise.all([
      import('./module1.js'),
      import('./module2.js'),
      import('./module3.js'),
    ]);
}
main();
```