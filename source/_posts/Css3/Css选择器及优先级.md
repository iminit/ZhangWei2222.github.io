---
title: Css选择器及优先级
date: 2019-03-05 09:15:37
categories:
- Css3
tags:
comments: false
---

## 前言

本篇讨论 css 的选择器、优先级算法以及选择器的优先级顺序

<!-- more -->

## 基本选择器

- id选择器（#myid）
- 类选择器（.myclassname）
- 标签选择器(（div, h1, p）
- 相邻选择器（h1 + p）、子选择器（ul > li）、后代选择器（li a）
- 通配符选择器（*）
- 属性选择器（a[rel=”external”]）
- 伪类选择器（a:hover/link/visited/active、li:nth-child）



## 优先级算法

每个规则对应一个初始四位数：0、0、0、0

- 若为 **内联**选择器，加 **1、0、0、0**
- 若为 **ID**选择器，加 **0、1、0、0**
- 若为 **类、伪类、属性**选择器，加  **0、0、1、0**
- 若为 **元素、伪元素**选择器，加 **0、0、0、1**

- **通配选择器、子选择器、相邻选择器** 都为 **0、0、0、0**

**算法：**将选择器对应的四位数相加，从左到右比较，大的优先级高



## 优先级

**注意：**

- **!important 的优先级是最高的**，但出现冲突时还是需要比较四位数

- 优先级相同，则采用就近原则，选择最后出现的样式

- **继承的属性，优先级最低**

  

**!important > 内联样式>ID选择器 > 类|伪类|属性选择器 > 元素|伪元素选择器 > 通配符 > 继承 > 浏览器默认属性**

**内联样式 > 内嵌样式 > 外联/导入样式**  



## 伪类 与 伪元素

#### 伪类

某个类的状态

- 链接伪类
- 【:link】
  - 【:visited】
- 动态伪类

  - 【:focus】
- 【:hover】
  - 【:active】
- 静态伪类

  - 【:first-child】选择元素的第一个子元素。


#### 伪元素

DOM 不存在的元素，只在 CSS 中用来渲染，添加一些特殊效果

- 首字母和首行
  - 【:first-letter】
  - 【:first-line】

- 之前和之后
  - 【:before】
  - 【:after】

#### 经常见到::after和:after，为什么会出现单冒号和双冒号？

单冒号 => CSS3 伪类，双冒号 => CSS3 伪元素

> 在 CSS3 中引入伪元素不再允许使用单冒号的写法，因此以后在某元素前后插入内容，**要用双引号**。