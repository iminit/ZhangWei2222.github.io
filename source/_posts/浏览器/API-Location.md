---
title: API-Location
date: 2019-11-07 18:13:37
categories:
- 浏览器
tags:
comments: false
---



## 前言

记录 `Location API`的属性值

<!-- more -->  



## 属性

- origin 属性
  - 返回URL的协议，主机名和端口号。可读

```js
假设当前的URL是https://www.jc2182.com:4088/test.htm#part2：
结果将是:https://www.jc2182.com:4098
```

- hash 属性
  - 一个可读可写的字符串，该字符串是 URL 的锚部分（从 # 号开始的部分

```js
假设当前的 URL 是 http://www.runoob.com/test.htm＃PART2：
结果将是:#part2
```