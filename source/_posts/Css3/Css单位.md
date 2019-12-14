---
title: Css 单位
date: 2017-08-18 15:15:47
categories:
- Css3
tags:
- 单位
comments: false
---

## 前言

除了平时经常使用的 px 、rem，Css 其实还提供了很多单位，接下来看一看吧~

<!-- more -->

## px

- **像素**
- 一个**固定大小**的单元



## rem

-  CSS3 新增，**相对于根元素 html** 文字大小
- 本质是等比缩放
- 使用：根元素 html 中写固定像素或百分比：1rem = 16px，再在具体元素上设置rem



## em

- **相对于父元素** 文字大小
  - 如果父元素设置了字体大小，1em = 1/父元素文字大小；没有设置，则使用浏览器 body 默认字体大小 16px
  - 如果自身设置了字体大小，em 相对于自身，1em = 1/自身字体大小



## vw 与 vh

- 视窗宽度 与 视窗高度

  - 1vw = 视窗宽度的 1%
  - 1vh = 视窗高度的 1%

- ie11 以上都兼容



## vmin 与 vmax

- vmin：vm、vh 较小的那个
- vmax：vm、vh 较大的那个
- ie11不兼容



## 其它

| 单位      | 描述                                                         |
| --------- | ------------------------------------------------------------ |
| %         | 百分比                                                       |
| in        | 寸                                                           |
| cm        | 厘米                                                         |
| mm        | 毫米                                                         |
| pt: point | 大约 1/72 寸                                                 |
| pc: pica  | 大约 6 pt, 1/6 寸                                            |
| ex        | 以“x”字符为基准<br />在无法确定x高度的情况下以0.5em计算<br />IE11及以下均不支持，firefox/chrome/safari/opera/ios safari/android browser4.4+等均需属性加么有前缀 |
| ch        | 以“0”字符为基准<br />找不到时为0.5em<br />ie10+,chrome31+,safair7.1+,opera26+,ios safari 7.1+,android browser4.4+支 |