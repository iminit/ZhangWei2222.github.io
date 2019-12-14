---
title: JS模块化区别：CommonJs、AMD、CMD、UMD、Es6Module
date: 2019-05-29 16:31:37
categories:
- JavaScript
tags:
- 模块化
comments: false
---

## 前言
之前在探索Es6 中import()和import...form的区别时，就在想Es6之前是咋引用模块的呀？？咱今天就来一探究竟！

## JS-模块化进程
Js在模块化上有经历一个越来越好的过程，目的是更好的封装模块，引用模块

原始的开发方式 -> CommonJs -> AMD -> CMD -> UMD -> Es6Module

## 原始的开发方式

### 例子
最开始的时候，Js自身是没有模块机制的。项目中有多个js文件，在html加载的时候，需要一个个文件引用 

```javascript
<script src="a.js"></script>
```

### 问题
1. 命名问题：所有的文件都是挂在 **global** 上，咱就不能重名了，否则污染全局环境。
1. 依赖问题：scrpit是顺序加载，如果各文件之间有依赖，比如某个a文件里面的代码依赖jquery，那jquery就必须放在a文件前面引用。我们需要时刻考虑js文件的书写顺序。
1. 网络问题：文件太多了，请求次数就会多，加载时间自然而然多！

突然想起最开始自己写的代码，都是原生js，或者使用jq，当时就经常会遇到重名、顺序，还有万恶的堵塞问题，嘤嘤嘤，模块化快出现！

## CommonJs

### 使用场景、加载方式
主要运行于服务器端（Node.js实现）

同步加载模块，因为加载的文件资源大多数在本地服务器，所以执行速度和时间都很棒

### 例子
该规范指出，一个单独的文件就是一个模块。

#### 主要两个命令

| 引用其他模块 | 输出                                                         |
| ------------ | ------------------------------------------------------------ |
| require      | module.exports/exports 注意：输出的是一个值得拷贝，输出后不能改变，会缓存起来 |


```javascript
// moduleA.js
var name = 'weiqinl'
function foo() {}

module.exports = exports = {
name,
foo
}

// moduleB.js
var ma = require('./moduleA') // 可以省略后缀.js
exports.bar = function() {
ma.name === 'weiqinl' // true
ma.foo() // 执行foo方法
}

// moduleC.js
var mb = require('./moduleB')
mb.bar()
```

## AMD
### 使用场景、加载方式
主要运行于浏览器端（Require.js实现）

异步加载，不会影响后面语句的执行


### 例子
一个单独的文件就是一个模块，模块与模块的依赖可以异步加载

#### 主要有4个命令

| 引用其他模块 | 定义                                                 | 输出接口 | 对象                                           |
| ------------ | ---------------------------------------------------- | -------- | ---------------------------------------------- |
| require      | define:全局函数，define(id?, dependencies?, factory) | return   | define.amd 属性是一个对象，表明函数遵循AMD规范 |


```javascript
// moduleA.js
define(['jQuery','lodash'], function($, _) {
    var name = 'weiqinl',
    function foo() {}
    return {
        name,
        foo
    }
})

// index.js
require(['moduleA'], function(a) {
    a.name === 'weiqinl' // true
    a.foo() // 执行A模块中的foo函数
    // do sth...
})

// index.html
<script src="js/require.js" data-main="js/index"></script>
```

## CMD
### 使用场景、加载方式
主要在浏览器运行，也可以在node.js运行（主要是Sea.js推广中形成的）

依赖就近，延迟执行。当程序执行到，才会自动加载

### 例子

#### 主要有4个命令
| 引用其他模块 | 定义                      | 输出接口 | 使用        |
| ------------ | ------------------------- | -------- | ----------- |
| require      | define:全局函数，定义模块 | exports  | seajs.use() |


```javascript
// moduleA.js
// 定义模块
define(function(require, exports, module) {
    var func = function() {
        var a = require('./a') // 到此才会加载a模块
        a.func()
        if(false) {
            var b = require('./b') // 到此才会加载b模块
            b.func()
        }
    }
    // do sth...
    exports.func = func;
})

// index.js
// 加载使用模块
seajs.use('moduleA.js', function(ma) {
    var ma = math.func()
})

// HTML，需要在页面中引入sea.js文件。
<script src="./js/sea.js"></script>
<script src="./js/index.js"></script>
```

## UMD

### 使用场景
通用模块定义 模式，主要用来解决CommonJs和AMD代码不能通用的问题，并同时还支持老式的全局变量规范

一般用于webpack打包，output.libraryTarget 将模块以哪种规范的文件输出

### 例子
```javascript
// 使用Node, AMD 或 browser globals 模式创建模块
(function (root, factory) {
if (typeof define === 'function' && define.amd) {
    // AMD模式. 注册为一个匿名函数
    define(['b'], factory);
} else if (typeof module === 'object' && module.exports) {
    // Node等类CommonJS的环境
    module.exports = factory(require('b'));
} else {
    // 浏览器全局变量 (root is window)
    root.returnExports = factory(root.b);
}
}(typeof self !== 'undefined' ? self : this,function (b) {
    // 以某种方式使用 b

    //返回一个值来定义模块导出。(即可以返回对象，也可以返回函数)
    return {};
}));
```

**解析**

1、判断 define 是否为函数，并是否存在 define.amd => AMD规范

2、判断 module 是否为一个对象，并是否存在 module.exports => CommonJs规范

3、如果以上两种都没有，设定为原始的代码规范

## Es6Module
### 注意的几个点
1、同时兼容node环境，可以和CommonJs混合使用。

2、因为可以和CommonJs混合使用，有两点区别

| CommonJs                                       | Es6                                                          |
| ---------------------------------------------- | ------------------------------------------------------------ |
| 运行时加载确定输出接口                         | 编译时确定输出接口                                           |
| 输出的是一个值的拷贝，输出后不能修改，会被缓存 | 输出的是一个值的引用，加载的时候会做静态优化（即模块内发生变化，import出的接口也会相应发生变化） |


3、对于加载外部模块
- 模块之中，import命令加载其他模块，.js后缀不可省略
- 模块之中，顶层的this返回undefined，而不是window
- 同一个模块加载多次，讲只执行一次

### 主要有两个命令

| 引用其他模块 | 输出                  |
| ------------ | --------------------- |
| import       | export/export default |

### 加载方式
#### 浏览器加载
1、外链js文件
    <script type="module" src="index.js"></script>

2、内嵌到内页

```javascript
<script type="module">
    import utils from './utils.js';
    // other code
</script>
```


## 名词区别

### 加载方式 区别
| 名词 | 同步加载                                           | 异步加载                                           | 依赖就近，延迟执行                          |
| ---- | -------------------------------------------------- | -------------------------------------------------- | ------------------------------------------- |
| 解释 | 需要等到依赖模块加载完成才编译执行(CommonJs)       | 哪一个模块小，就先执行哪一个(AMD)                  | 当需要某个模块的时候再去再去异步加载（CMD） |
| 不足 | 阻塞加载，当依赖的模块太大，浏览器会处于假死的状态 | 提前加载，依赖前置（要用到的模块，一开始就要写好） |                                             |


```javascript
// AMD 默认推荐的是
define(['./a', './b'], function(a, b) { // 依赖必须一开始就写好
    a.doSomething()
    // 此处略去 100 行
    b.doSomething()
    ...
})

// CMD
define(function(require, exports, module) {
    var a = require('./a')
    a.doSomething()
    // 此处略去 100 行
    var b = require('./b') // 依赖可以就近书写
    b.doSomething()
    // ... 
})
```

### import、require、import()区别 


| require                                                      | import                                              | import()                           |
| ------------------------------------------------------------ | --------------------------------------------------- | ---------------------------------- |
| CommonJs(moudule.exports/exports)、AMD(return)、CMD(exports)都用这个命令加载 | es6加载(export/export.default)                      | 同前                               |
| 运行时调用，可以随处引入                                     | 编译时调用，必须放到文件开头，需要babel把es6转成es5 | 运行时调用，可以随处引入，异步加载 |
| 引入基础数据类型时，属于复制；引用复杂数据类型时，属于浅拷贝（值的拷贝） | 值的引用，会跟随引入的模块一起变化                  | 同前                               |

### 运行时加载、编译时加载区别
#### 运行时加载
默认export的是一个对象，输入时必须查找对象属性。

```javascript
// CommonJS模块
let { stat, exists, readFile } = require('fs');

// 等同于
let _fs = require('fs');
let stat = _fs.stat;
let exists = _fs.exists;
let readfile = _fs.readfile;
```

上面代码的实质是整体加载fs模块（即加载fs的所有方法），生成一个对象（_fs），然后再从这个对象上面读取 3 个方法。这种加载称为“运行时加载”，因为只有运行时才能得到这个对象，导致 **完全没办法在编译时做“静态优化”**

#### 编译时加载
ES6 模块不是对象，而是通过export命令显式指定输出的代码，再通过import命令输入。

```javascript
import { stat, exists, readFile } from 'fs';
```

上面代码的实质是从fs模块加载 3 个方法，其他方法不加载。这种加载称为“编译时加载”或者静态加载，即 ES6 可以在编译时就完成模块加载，效率要比 CommonJS 模块的加载方式高

## 总结
| -                | CommonJs                           | AMD                            | CMD                     | ES6                                                          |
| ---------------- | ---------------------------------- | ------------------------------ | ----------------------- | ------------------------------------------------------------ |
| 使用场景         | 服务器端:Nodejs                    | 浏览器端:Require.js            | 服务器浏览器皆可:Sea.js | 服务器浏览器皆可,还可以和CommonJs混用                        |
| 定义             |                                    | define                         | define                  |                                                              |
| 暴露接口         | moudlue.exports/exports            | return                         | exports                 | export/exportdefalut                                         |
| 引用             | require                            | require                        | require                 | import...form/import()                                       |
| 引用语句加载方式 | 运行时加载（那会才知道依赖的关系） | 运行时加载                     | 运行时加载              | 编译时加载(进行值的引用，可以静态优化，会跟随引入的模块一起变化；前面几种进行的是值的拷贝，输出后不能改变，得到的是缓存的结果)，静态加载，只能放在代码的最前面；但是import()为运行时加载，异步加载 |
| 依赖加载方式     | 同步加载                           | 异步加载(提前加载，依赖前置)载 | 依赖就近，延迟执行      |                                                              |
| 注意             |                                    |                                |                         | es6-es5的转译，需要有babel，使用import()还需要额外的babel配置 |