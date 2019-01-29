---
title: stylus相关和1像素边框问题
date: 2017-09-09 15:15:47
categories:
- 前端技术篇
tags:
- VUE
- 饿了么
comments: false
---

### 1像素边框问题
1像素边框问题其实就是Dpr的比例问题,例如 Retina屏设置1px边框，实际显示2px,因为Dpr是2,所以会显示为2px

目前这里用的是伪元素 + transform 实现:

伪元素:总结伪类与伪元素


```
::after 在某元素之后插入某些内容
::before 在某元素之前插入某些内容
```

> 原理是利用 :before 或者 :after 重做 border,并且根据媒体查询设置不同的缩放比例(transform 的 scale)
> 具体操作会在stylus里面实现

### stylus相关
- 使用stylus的写法,通过缩进表示层次与嵌套关系
    - 使用mixins方法来更方便的组件化css,相关写法可以参考:官网的mixins
- 创建/sell/src/common/stylus这个目录,用来存放stylus相关的文件


```
src/common/stylus
├── base.styl
├── icon.styl
├── index.styl
└── mixin.styl
```

以下分别是四个文件的代码(名字只是为了区分不同的作用):

#### base.styl

```
<!--这里是对于使用reset.css之后的一些自定义的默认的css初始化-->
<!--字体是一些移动端比较流行的字体,所以这里使用-->
<!--其他都是为了方便开发,避免css的属性继承影响模块的代码-->
body, html
  line-height: 1
  font-weight: 200
  <!--设置一些移动端的常用的字体-->
  font-family: 'PingFang SC', 'STHeitiSc-Light', 'Helvetica-Light', Arial, sans-serif
<!--根据媒体查询@media设置不同的缩放比例(transform 的 scale)来修复1像素边框的问题-->
@media (-webkit-min-device-pixel-ratio: 1.5),(min-device-pixel-ratio: 1.5)
  .border-1px
  <!--使用伪元素::after--> 
    &::after 
    <!--dpr是1.5的时候缩放0.7倍-->
      -webkit-transform: scaleY(0.7) 
      transform: scaleY(0.7)

@media (-webkit-min-device-pixel-ratio: 2),(min-device-pixel-ratio: 2)
  .border-1px
    &::after
    <!--dpr是2的时候缩放0.5倍-->
      -webkit-transform: scaleY(0.5) 
      transform: scaleY(0.5)
```

- 这里的修复1像素边框问题会拆分为2个部分,一个部分是这里的base.styl里面处理缩放,另外一部分是在mixin.styl里面处理重做border
- 这里是一个base模块文件,只保留了基本的共用的css,需要结合其他的css文件(stylus)来合并理解
- dpr一般是1或者2,1.5只是为了更精细的去适配1和2之间的手机型号


#### mixin.styl

```
//这里是负责1像素边框问题
border-1px($color) //stylus的mixins语法支持传入变量
  position: relative //创建相对定位布局,为after和before的border的绝对布局做定位
  &::after //使用伪元素::after,在元素之后插入内容
    display: block
    position: absolute
    left: 0
    bottom: 0
    width: 100% //需要横向撑开边框宽度
    border-top: 1px solid $color //因为是在元素之后插入,所以是用上边框
    content: ' ' //after或者before使用的时候,不传入东西也要填一个空字符串

//这里是负责图片的不同dpr下显示高清问题
bg-image($url) //这是图片的mixin,也是类似,不过这里是图片根据dpr来进行适配
  background-image: url($url + "@2x.png")
  @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3)
    background-image: url($url + "@3x.png")
```

- 这里的border-1px($color) 就是真正处理1像素边框问题的关键,通过伪元素after重做border,并且支持传入颜色变了$color来自定义颜色
- 这里的bg-image($url) 是负责处理图片在不同dpr下显示的问题,原来跟1像素边框问题差不多,不过这里不需要重做,只是根据不同的media query来调用不同的图片显示,而这些图片是需要放在相对于的文件夹的,在这个项目的话,是放在.vue文件的文件夹下.

#### icon.styl
这个其实是用icomoon生成的icon样式文件,通过将小图片转为字体,然后通过css样式调用,这样做的目的可以减少小图片的http请求,也方便管理和使用这些小图片,例如可以直接使用某个图片的class就可以调用该图片


```
<!--这个其实就是之前图标字体生成的css文件改过来的-->
<!--因为使用webpack打包的关系,需要修改一下url路径,这个按照当前目录查找到文件就可以了-->
@font-face
  font-family: 'sell-ico'
  src: url('../fonts/sell-ico.eot?gr00o7')
  src: url('../fonts/sell-ico.eot?gr00o7#iefix') format('embedded-opentype'),
    url('../fonts/sell-ico.ttf?gr00o7') format('truetype'),
    url('../fonts/sell-ico.woff?gr00o7') format('woff'),
    url('../fonts/sell-ico.svg?gr00o7#sell-ico') format('svg')
  font-weight: normal
  font-style: normal

[class^="icon-"], [class*=" icon-"]
  /* use !important to prevent issues with browser extensions that change fonts */
  font-family: 'sell-ico' !important
  speak: none
  font-style: normal
  font-weight: normal
  font-variant: normal
  text-transform: none
  line-height: 1

  /* Better Font Rendering =========== */
  -webkit-font-smoothing: antialiased
  -moz-osx-font-smoothing: grayscale

.icon-arrow_lift:before
  content: "\e900"

.icon-thumb_up:before
  content: "\e901"

.icon-thumb_down:before
  content: "\e902"

.icon-shopping_cart:before
  content: "\e903"

.icon-favorite:before
  content: "\e904"

.icon-check_circle:before
  content: "\e905"

.icon-close:before
  content: "\e906"

.icon-remove_circle_outline:before
  content: "\e907"

.icon-add_circle:before
  content: "\e908"

.icon-keyboard_arrow_right:before
  content: "\e909"
index.styl
使用一个主的styl来包含所有的stylus文件

@import "./icon" 
@import "./mixin"
@import "./base"
```
