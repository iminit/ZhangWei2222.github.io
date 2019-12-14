---
title:  Display 属性
date: 2019-03-04 12:12:37
categories:
- Css3
tags:
- Display
comments: false
---

## 前言

display 属性值有： inherit、none、inline、block、inline-block、list-item(作为列表显示)、run-in(根据上下文作为块级元素或内联元素显示)、table及其相关的一系列值。下面我们讨论前面几个最常见的属性值

<!-- more -->

## none

- **隐藏**，元素不会显示
- 相较与 visibility: hidden 
  - 它会脱离文本流；把元素空间也隐藏掉；引发重绘，回流
  - 而 hidden 不会脱离文本流；会保留元素空间；引发重绘 



## inline

- **默认值，内联元素（行内元素）**
- 与其他元素在一行上
- 高度、宽度、内边距不可控；宽高就是内容的宽高
- 只能包含行内元素，不能包含块级元素

```html
<span></span>
<br> 换行
<a></a> 链接
<b></b> 加粗
<strong></strong> 加粗
<sup></sup> 上标
<sub></sub> 下标
<i></i> 斜体
<em></em> 斜体
<del></del> 删除线
<u></u> 下划线
<input> 文本框
<textarea></textarea> 多行文本
<select></select> 下拉列表
<img> 图片
```



## block

- **块级元素**
- 元素独占一行
- 宽高可控；宽度没有设置时，为100%
- 可以包含块级与行内元素

```html
<div></div>
<p></p> 段落
<h1></h1> 标题等 
<ul></ul> 无序列表
<ol></ol> 有序列表
<dl></dl> 定义列表
<li></li>
<hr> 水平分割线
<table></table> 表格
<form></form> 表单
<pre></pre> 预格式化
<blockquote></blockquote> 段落缩进，前后5个字符
<address></address> 地址
<center></center> 
```



## inline-block

- **行内块元素**
- 融合了 inline 和 block 特性，即是内联元素，又可以设置宽高