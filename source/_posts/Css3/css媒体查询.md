---
title: css媒体查询
date: 2017-08-18 15:15:47
categories:
- Css3
tags:
comments: false
---

### 1.语法

```html
<!-- link元素中的CSS媒体查询 -->
<link rel="stylesheet" media="(max-width: 800px)" href="example.css" />

<!-- 样式表中的CSS媒体查询 -->
<style>
@media (max-width: 600px) {
  .facet_sidebar {
    display: none;
  }
}
</style>
```

即使媒体查询返回false, 标签指向的样式表也将会被下载(但是它们不会被应用)

<!-- more -->

### 2.逻辑操作符
#### (1).and

```
@media tv and (min-width: 700px) and (orientation: landscape) { ... }
```

现在，上面媒体查询仅在电视媒体上，可视区域不小于700像素宽度并且是横屏时有效。

#### (2).逗号分隔列表

```
@media (min-width: 700px), handheld and (orientation: landscape) { ... }
```

等同于 or 逻辑操作符。

#### (3).not

```
@media not all and (monochrome) { ... }
@media not (all and (monochrome)) { ... }

@media not screen and (color), print and (color)
@media (not (screen and (color))), print and (color)
```

#### (4).only
only 关键字防止老旧的浏览器不支持带媒体属性的查询而应用到给定的样式：


```
<link rel="stylesheet" media="only screen and (color)" href="example.css" />
```

### 3.媒体属性
#### (1).颜色（color）
指定输出设备每个像素单元的比特值。如果设备不支持输出颜色，则该值为0。


```
向所有能显示颜色的设备应用样式表：

@media all and (color) { ... }

向每个颜色单元至少有4个比特的设备应用样式表：

@media all and (min-color: 4) { ... }
```

#### (2).颜色索引（color-index）
向所有使用索引颜色的设备应用样式表
```
，你可以这么做：

@media all and (color-index) { ... }
向所有使用至少256个索引颜色的设备应用样式表：

<link rel="stylesheet" media="all and (min-color-index: 256)" href="http://foo.bar.com/stylesheet
```

#### (3).宽高比（aspect-ratio）
描述了输出设备目标显示区域的宽高比。该值包含两个以“/”分隔的正整数。代表了水平像素数（第一个值）与垂直像素数（第二个值）的比例。

#### (4).设备宽高比（device-aspect-ratio）
描述了输出设备的宽高比。该值包含两个以“/”分隔的正整数。代表了水平像素数（第一个值）与垂直像素数（第二个值）的比例。


```
@media screen and (device-aspect-ratio: 16/9), screen and (device-aspect-ratio: 16/10) { ... } 宽高比或者16：9或者16：10。
```

#### (5).设备高度（device-height）
描述了输出设备的高度（整个屏幕或页的高度，而不是仅仅像文档窗口一样的渲染区域）。

向显示在最大宽度800px的屏幕上的文档应用样式表，你可以这样做：


```
<link rel="stylesheet" media="screen and (max-device-width: 799px)" />
```

#### (6).设备宽度（device-width）
描述了输出设备的宽度（整个屏幕或页的高度，而不是仅仅像文档窗口一样的渲染区域）。

#### (7).网格（grid）
是否接受 min/max 前缀： 否

判断输出设备是网格设备还是位图设备。如果设备是基于网格的（例如电传打字机终端或只能显示一种字形的电话），该值为1，否则为0。

#### (8).高度（height）
height 媒体属性描述了输出设备渲染区域（如可视区域的高度或打印机纸盒的高度）的高度。

#### (9).黑白（monochrome）
指定了一个黑白（灰度）设备每个像素的比特数。如果不是黑白设备，值为0。

#### (10).方向（orientation）
值：landscape | portrait

指定了设备处于横屏（宽度大于高度）模式还是竖屏（高度大于宽度）模式。

向竖屏设备应用样式表：


```
@media all and (orientation: portrait) { ... }
```

#### (11).分辨率（resolution）
指定输出设备的分辨率（像素密度）。分辨率可以用每英寸（dpi）或每厘米（dpcm）的点数来表示。

#### (12).扫描（scan）
值： progressive | interlace

向以顺序方式扫描的电视机上应用样式表：


```
@media tv and (scan: progressive) { ... }
```

#### (13).宽度（width）
width 媒体属性描述了输出设备渲染区域（如可视区域的宽度或打印机纸盒的宽度）的宽度。

