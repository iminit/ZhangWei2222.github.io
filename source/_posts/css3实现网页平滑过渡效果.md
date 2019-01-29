---
title: css3实现网页平滑过渡效果
date: 2017-10-17 15:15:47
categories:
- 前端技术篇
tags:
- css3
comments: false
---

### 一、如何用纯css判断盒子被选中？
用radio

### 二、font-smoothing
- 功能：css3中用于设置字体的抗锯齿或者说光滑度的属性
- 语法规则：
    - font-smoothing:subpixel-antialiased | none | antialiased
- 取值说明
     - none用于小像素的文本、subpixel-antialiased浏览器默认的、antialiased反锯齿

### 三、translate3d
可以开启GPU硬件加速

### 四、backface-visibility
- 功能：定义当元素不面向屏幕时是否可见
- 语法规则：
    - backface-visibility:visible | hidden
- 取值说明：
    - visible:背面是可见的
    - hidden:背面是不可见的

### 五、图标三角背景制作
当画一个大三角形的时候，可以先用css画一个大矩形，再旋转，最后放到合适的地方露出直角，隐藏其他

### 六、怎么实现动画过渡
当写了transform时，动画是一下子变得，没有过渡。要实现过渡，需要在变换的盒子上加上transition

### 七、怎么实现网页平滑过渡效果

```
/* 内容 */
.st-scroll,
.st-panel{
    width: 100%;
    height:100%;
    position: relative; 
}

.st-scroll{
    left: 0;
    top: 0;
    -webkit-transform: translate3d(0,0,0);
    -moz-transform: translate3d(0,0,0);
    -o-transform: translate3d(0,0,0);
    -ms-transform: translate3d(0,0,0);
    transform: translate3d(0,0,0);
    -webkit-backface-visibility: hidden;
    /* 过渡谁就写谁 */
    -webkit-transition: all 0.6s ease-in-out; 
    -moz-transition: all 0.6s ease-in-out;
    -o-transition: all 0.6s ease-in-out;
    -ms-transition: all 0.6s ease-in-out;
    transition: all 0.6s ease-in-out;
}

.st-panel{
    background: #fff;
    overflow: hidden;
}

#st-control-1:checked ~ .st-scroll{
    -webkit-transform: translateY(0%);
    -moz-transform: translateY(0%);
    -o-transform: translateY(0%);
    -ms-transform: translateY(0%);
    transform: translateY(0%);
}
```

.st-scroll 是全部内容，.st-panel是单个内容。核心思想是，让每个内容100%占据网页，然后通过选择导航栏，让scroll移动（translateY)

### 八、@font-face实现小icon
所有你要出现图标的div，都要加上data-icon的属性。里面的值就是显示的东西。css中用属性选择器进行书写，属性选择器要加[]


```
<div class="st-desc" data-icon="C"></div>

[data-icon]:after{
    content: attr(data-icon);
}
```

#### @font-face
- 功能
    - @font-face 是css3中的一个模块，主要用于将自己定义的Web字体嵌入到网页中
- 语法规则
    
```
 @font-face{
    font-family : <YourWebFontName>;
    src : <source> [<format>][,<source>[<format>]]*;
    [font-weight:<weight>];
    [font-style:<style>];
}
```

- 取值说明
    - source:此值指的是你自定义的字体的存放路径，可以是相对路径也可以是绝对路径；
    - format:此值指的是你自定义的字体的格式，主要用来帮助浏览器识别，其值主要有以下几种类型：
    - truetype,opentype,truetype-aat,embedded-opentype,svg等；
- 常用的几种字体格式
    - TureType(.ttf)格式
    - OpenType(.otf)格式
    - Web Open Font Format(.woff)格式
    - Embedded Open Type(.eot)格式
    - SVG(.svg)格式
- 字体下载
    - 网址：http://www.dafont.com/
    - 网址：http://www.google.com/webfonts

### 九、transition && animation 区别
transition 必须要事件触发，而animation用不用事件触发都可以

