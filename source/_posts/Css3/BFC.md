---
title:  BFC
date: 2019-03-07 18:12:37
categories:
- Css3
tags:
comments: false
---

## 定义

- 块级格式化上下文
- 一个独立的布局环境，其中的元素布局不受外界影响
- 在一个BFC中，块盒与行盒（行盒由一行中所有的内联元素所组成）都会垂直的沿着其父元素的边框排列



## 布局规则

1. 内部的 Box 会在垂直方向，一个接一个地放置
2. Box 垂直方向的距离由 margin 决定，**属于同一个 BFC 的两个相邻 Box 的 margin 会发生重叠**
3. 每个元素的 margin box 的左边， 与包含块 border box 的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此
4. BFC 的区域**不会与 float box 重叠**
5. BFC 是页面上的一个隔离的独立容器，容器里面的**子元素不会影响到外面的元素**。**反之也如此**。
6. 计算 BFC 的高度时，浮动元素也参与计算



## 创建BFC，满足下列的任意一个或多个条件即可

1. float 的值不是 none
2. position 的值不是 static 或者 relative
3. overflow 的值不是 visible
4. display 的值是 inline-block、table-cell、flex、table-caption 或者 inline-flex