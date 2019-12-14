---
title:  Position 属性
date: 2019-03-04 10:12:37
categories:
- Css3
tags:
- Position
comments: false
---

## 前言
还记得刚开始入门前端的时候，总是被 position 弄得脑袋一团乱，现在打码多了，总算有点思路了。还是需要再理论巩固一下！

<!-- more -->

## inherit

- **继承父元素**的 position 属性
- IE8及之前的版本不支持该属性

## static

- 默认值，**没有定位**
- 元素处于正常的文档流中，忽略 left、right、top、bottom 和 z-index 属性

## relative

- **相对定位**，相对原本位置的定位
- 元素不脱离文档流，原本的位置会保留，其他元素位置不受影响

## absolute

- **绝对定位**
- 相对于 static 定位以外的第一个父元素定位；如果没有，则相对于 body 定位
- 元素脱离文档流，原本的位置会被后面的元素填充

## fixed

1. **绝对定位**，可以说是特殊的 absolute
2. 相对于 浏览器窗口 定位，不随滚动条改变位置

## sticky

- **粘性定位**，CSS3 新发布的一个属性，兼容性不好，只支持 FixFox 和 Safari
- 屏幕范围内不受定位影响，元素将要移出偏移范围，定位会变成 fixed ，根据设置的 left、top 等属性偏移

## 拓展

#### z-index

只在有定位的元素生效（relative、aboslute、fixed..）