---
title: Node.js-path模块中 path.resolve() 和 path.join() 的区别
date: 2020-04-28 11:29:37
categories:
- Node.js
comments: false
---

### path 模块的引入

**作用：**直接引用，node中自带的模块

```js
const path = require('path');
```

<!-- more -->



### path.join(path1，path2，path3.......)

**作用：**将路径片段使用特定的分隔符（window：\）连接起来形成路径，并规范化生成的路径。若任意一个路径片段类型错误，会报错。

```js
const path = require('path')
let myPath = path.join(__dirname, '/client/src')
let myPath2 = path.join(__dirname, './client/src')
let myPath3 = path.join('/foo', 'bar', 'baz/s', '..')

console.log(__dirname) // I:\Youth-Hostel
console.log(myPath)    // I:\Youth-Hostel\client\src
console.log(myPath2)   // I:\Youth-Hostel\client\src
console.log(myPath3)   // \foo\bar\baz
```



### path.resolve([from...],to)

**作用：**把一个路径或路径片段的序列解析为一个绝对路径。相当于执行 cd 操作。

`/` 被解析为根目录。

```js
const path = require('path')
let myPath = path.resolve(__dirname, '/client/src')
let myPath2 = path.resolve(__dirname, './client/src')
let myPath3 = path.resolve('/foo', '/bar')
let myPath4 = path.resolve('/foo', './bar')

console.log(__dirname) // I:\Youth-Hostel
console.log(myPath)    // I:\client\src
console.log(myPath2)   // I:\Youth-Hostel\client\src
console.log(myPath3)   // I:\bar
console.log(myPath4)   // I:\foo\bar
```

