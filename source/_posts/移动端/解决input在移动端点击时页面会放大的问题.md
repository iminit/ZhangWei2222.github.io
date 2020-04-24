---
title: 解决input在移动端点击时页面会放大的问题
date: 2020-04-17 17:13:47
categories:
- 移动端
comments: false
---

在毕设项目（移动项目）中，发现真机测试时，点击输入框，页面会放大，以下为解决办法：

```html
<!-- Vue项目中的index.html中加 -->
<meta name="viewport" content="width=device-width, initial-scale=1, minimum-scale=1, maximum-scale=1"/>
```

