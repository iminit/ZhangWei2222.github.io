---
title: vue动画效果配置和弹层css sticky footer
date: 2017-09-09 15:15:47
categories:
- Vue
tags:
- 饿了么教程
comments: false
---

### vue动画效果配置
有两种方式:

- 一种是css方式
需要注意 4 个(CSS)类名在 enter/leave 的过渡中切换:

1. v-enter: 定义进入过渡的开始状态。在元素被插入时生效，在下一个帧移除。
1. v-enter-active: 定义进入过渡的结束状态。在元素被插入时生效，在 transition/animation 完成之后移除。
1. v-leave: 定义离开过渡的开始状态。在离开过渡被触发时生效，在下一个帧移除。
1. v-leave-active: 定义离开过渡的结束状态。在离开过渡被触发时生效，在 transition/animation 完成之后移除。

一般的使用方式是:
在html里面用transition包裹起来,然后建立一个name,这个name对应css的class名字的前缀,例如这里name是fade,class就是fade前缀命名

html代码


```
<transition name="fade">
</transition>
```

css代码


```
&.fade-enter-active, &.fade-leave-active
&.fade-enter, &.fade-leave-active
```

> 总的来说:在enter里面会代表状态从没有到有的变化,在leave里面会代表状态从有到没有的变化,在使用的时候可以按照需要使用,需要考虑好动画的变化再开始写代码

- 一种是js钩子的方式


```
methods: {
  // --------
  // 进入中
  // --------
  beforeEnter: function (el) {
    // ...
  },
  // 此回调函数是可选项的设置
  // 与 CSS 结合时使用
  enter: function (el, done) {
    // ...
    done()
  },
  afterEnter: function (el) {
    // ...
  },
  enterCancelled: function (el) {
    // ...
  },
  // --------
  // 离开时
  // --------
  beforeLeave: function (el) {
    // ...
  },
  // 此回调函数是可选项的设置
  // 与 CSS 结合时使用
  leave: function (el, done) {
    // ...
    done()
  },
  afterLeave: function (el) {
    // ...
  },
  // leaveCancelled 只用于 v-show 中
  leaveCancelled: function (el) {
    // ...
  }
}
```

一般的使用方式是


```
<transition @before-enter="beforeDrop" @enter="dropping" @after-enter="afterDrop">
</transition>
```

通过绑定js方法来实现

> 总的来说:js钩子会更加细腻,会在变化的过程中也加入一个钩子管理变化,其他跟css方式动画差不多

### 弹层和css sticky footer原理
css sticky footer解释:如果页面内容不够长的时候，页脚块粘贴在视窗底部；如果内容足够长时，页脚块会被内容向下推送。

> 这里主要使用clearfix这个类的伪类来完成.

原理简述如下:

- 在IE6, 7下zoom: 1会触发hasLayout，从而使元素闭合内部的浮动。
- 在标准浏览器下，.clearfix:after这个伪类会在应用到.clearfix的元素后面插入一个clear: both的块级元素，从而达到清除浮动的作用。
- 在需要清除浮动的时候，只要写一个.clearfix就行了，然后在需要清浮动的元素中 添加clearfix类名就好了
- 防止Collapsing margins(外边距折叠)
- 保证跨浏览器的兼容性

相关代码如下:


```
.clearfix:before, clearfix:after {
    content: ".";   
    display: block;   
    clear: both;     
    visibility: hidden;
    line-height: 0;    //行高为0
    height: 0;     //高度为0
    font-size:0;    //字体大小为0
}

.clearfix { *zoom:1;}   //这是针对于IE6的，因为IE6不支持:after伪类，这个神奇的zoom:1让IE6的元素可以清除浮动来包裹内部元素
```

备注:
- zoom: 1 为了兼容 IE6/7,在IE6, 7下zoom: 1会触发hasLayout，从而使元素闭合内部的浮动。
- :before 用来防止 top-margin collapse 和保证当 IE6/7 应用 zoom:1 时的是视觉一致性
- :after 用来清除浮动
    - visibility: hidden 可见度设为隐藏。注意它和display:none;是有区别的。visibility:hidden;仍然占据空间，只是看不到而已
    - clear: both 清除左右两边浮动
    - display: block需要将元素转为块级元素才可以实现效果,inline-block也可以
    - content: "." 内容为“.”就是一个英文的句号而已。也可以不写。
    - line-height: 0行高为0,设置这些主要是为了避免一些占用位置或者间隔的问题
    - height: 0高度为0,同上
    - font-size:0 字体大小为0,同上
> 另外需要注意的是避免Collapsing margins(外边距压扁)
> 有两种情况会引起 Collapsing margin:
> 
> - 父子元素(不一定是直接父子)之间会发生 margin-top 和 margin-bottom 的折叠 (如果之间没有 border、padding、inlne-content、height、min-height、max-height 分割的话)
> - 邻近元素同时设置margin,前面的margin-bottom 和后面的margin-top融合取最大的
>
> 所以这里能够实现避免Collapsing margins是因为
> - 父子的话,给父元素添加.clearfix
> - 紧邻的话,给前一个元素添加.clearfix
> 这样主要利用 :before psudo-element 给元素内部前面添加一个空的元素 display:block 保证它是一个BFC(BFC可以隔断外边距折叠)


#### 课程里面使用
因为clearfix是一个比较常用的class,所以在base.styl里面添加,方便其他组件调用


```
.clearfix
  display: inline-block
  &:before, &:after
    contain: ''
    height: 0
    line-height: 0
    clear: both
    visibility: hidden
```

这里没有去兼容ie6,7,因为移动端基本不包含这些浏览器
使用inline-block是因为做的是行内块处理

#### 在Header.vue里面使用


```
<div class="detail">
  <div class="detail-wrapper clearfix"></div>
  <div class="detail-close"></div>
</div>
```

- 先用一个div包裹其需要做css sticky的内容,如detail的div
- 然后footer是detail-close按钮,而需要显示并且要持续撑开的内容块是detail-wrapper,那么在detail-wrapper上添加一个clearfix 的class,就会在这个元素后面使用after的伪类生成内容,从而不断推开footer