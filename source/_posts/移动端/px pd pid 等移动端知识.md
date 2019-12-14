---
title: px pd pid 等移动端知识
date: 2017-08-18 15:15:47
categories:
- 移动端
tags:
- 单位
comments: false
---

### 1.PX（css pixels）——虚拟像素
px是一个相对单位，相对的是设备像素

显示器上的物理像素等于显示器的点距，而打印机的物理像素等于打印机的墨点。而衡量点距大小和打印机墨点大小的单位分别称为ppi和dpi：

ppi：每英寸多少像素数，放到显示器上说的是每英寸多少物理像素及显示器设备的点距。

dpi：每英寸多少点。

### 2.DP(device pixels)——设备像素（物理像素）
pt在css单位中属于真正的绝对单位，1pt = 1/72(inch),inch及英寸，而1英寸等于2.54厘米。

> 屏幕普遍采用RGB色域(红、绿、蓝三个子像素构成),而印刷行业普遍使用CMYK色域(青、品红、黄和黑)

### 3.设备像素(DP)与CSS像素之间的关系

```
DPR = 设备像素/CSS像素
```

当这个比率为1:1时，使用1个设备像素显示1个CSS像素。当这个比率为2:1时，使用4个设备像素显示1个CSS像素，当这个比率为3:1时，使用9（3*3）个设备像素显示1个CSS像素。

window对象有一个devicePixelRatio属性，它的官方的定义为：设备物理像素和设备独立像素的比例，也就是 devicePixelRatio = 物理像素 / 独立像素。

### 4.DIP(Device independent Pixel)
设备独立像素，也称为逻辑像素，简称dip。


```
CSS像素 =设备独立像素 = 逻辑像素
```


### 5.PPI(pixels per inch)
每英寸像素取值，更确切的说法应该是像素密度，也就是衡量单位物理面积内拥有像素值的情况。

### 6.ppi和dpr到底什么关系？
设备像素比与ppi相关，一般是ppi/160的整数倍：

### 7.分辨率、像素和屏幕尺寸
PPI 说的是像素密度，而分辨率说的是块屏幕的像素尺寸，譬如说 1334*750 就是 iPhone（6~7）的分辨率，说 iPhone（6~7）的分辨率是 326 是错误的表述，326 是它的像素密度，单位是 PPI。

### 8.Viewport
ppk认为，移动设备上有三个viewport。

- layout viewport document.documentElement.clientWidth 来获取
- visual viewport的宽度可以通过window.innerWidth 来获取
- viewport——移动设备的理想viewport。根据不同的设备有不同的宽度

##### (1).怎么让layout viewport=visual viewport？


##### (2).关于缩放以及initial-scale的默认值

```
visual viewport宽度 = ideal viewport宽度 / 当前缩放值
当前缩放值 = ideal viewport宽度 / visual viewport宽度
```

在iphone和ipad上，无论你给viewport设的宽的是多少，如果没有指定默认的缩放值，则iphone和ipad会自动计算这个缩放值，以达到当前页面不会出现横向滚动条(或者说viewport的宽度就是屏幕的宽度)的目的。

