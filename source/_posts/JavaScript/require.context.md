---
title: require.context
date: 2019-11-01 18:26:37
categories:
- JavaScript
tags:
comments: false
---



读取文件的路径,是否遍历文件的子目录,匹配文件的正则

遍历文件夹中的指定文件 然后自动导入 不需要每次显式的调用import导入模块 ，比如一个想要导入一个文件夹内的所有文件，可以不用一个个import

```js
const req = require.context('./floors/youpin-home', true, /\.vue$/im);
```

