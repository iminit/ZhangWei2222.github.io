---
title: jquery(四)动画篇
date: 2017-08-18 15:15:47
categories:
- JavaScript
tags:
- jQuery
comments: false
---

### 1.jQuery中隐藏元素的hide方法

```
.hide( options )
.hide("fast / slow")
```

隐藏有动画效果

### 2.jQuery中显示元素的show方法

```
.hide( options )
.hide("fast / slow")
```

注意事项：

如果使用!important在你的样式中，比如display: none !important，如果你希望.show()方法正常工作，必须使用.css(‘display’, ‘block !important’)重写样式

### 3.jQuery中显示与隐藏切换toggle方法

```
toggle();
toggle( [duration ] [, complete ] )
```

如果元素是最初显示，它会被隐藏
如果隐藏的，它会显示出来
### 4.jQuery中下拉动画slideDown
.slideDown()：用滑动动画显示一个匹配元素

.slideDown( [duration ] [, complete ] )

持续时间（duration）是以毫秒为单位的，数值越大，动画越慢，不是越快。字符串 ‘fast’ 和 ‘slow’ 分别代表200和600毫秒的延时。如果提供任何其他字符串，或者这个duration参数被省略，那么默认使用400 毫秒的延时。

具体使用：

```
$("ele").slideDown(1000, function() {
    //等待动画执行1秒后,执行别的动作....
});
```

### 5.jQuery中上卷动画slideUp

```
$("elem").slideUp();

.slideUp( [duration ] [, easing ] [, complete ] )
```

因为动画是异步的，所以要在动画之后执行某些操作就必须要写到回调函数里面，这里要特别注意

### 6.jQuery中上卷下拉切换slideToggle

```
slideToggle();
.slideToggle( [duration ] ,[ complete ] )
```

### 7.jQuery中淡出动画fadeOut

```
.fadeOut( [duration ], [ complete ] )
```

### 8.jQuery中淡入动画fadeIn

```
.fadeIn( [duration ], [ complete ] )
```

淡入的动画原理：操作元素的不透明度从0%逐渐增加到100%

### 9.jQuery中淡入淡出切换fadeToggle

```
.fadeToggle( [duration ] ,[ complete ] )
```

### 10.jQuery中淡入效果fadeTo

```
.fadeTo( duration, opacity ,callback)
```

相比fadeOut,fadeIn,可以改变透明度

### 11.toggle、sildeToggle以及fadeToggle的区别：
1. toggle：切换显示与隐藏效果
1. sildeToggle：切换上下拉卷滚效果
1. fadeToggle：切换淡入淡出效果

### 12.jQuery中动画animate(上)
.animate( properties ,[ duration ], [ easing ], [ complete ] )
参数分解：

- properties：一个或多个css属性的键值对所构成的Object对象。要特别注意所有用于动画的属性必须是数字的，除非另有说明；这些属性如果不是数字的将不能使用基本的jQuery功能。比如常见的，border、margin、padding、width、height、font、left、top、right、bottom、wordSpacing等等这些都是能产生动画效果的。background-color很明显不可以，因为参数是red或者GBG这样的值，非常用插件，否则正常情况下是不能只用动画效果的。注意，CSS 样式使用 DOM 名称（比如 “fontSize”）来设置，而非 CSS 名称（比如 “font-size”）。

特别注意单位，属性值的单位像素（px）,除非另有说明。单位em 和 %需要指定使用


```
.animate({
    left: 50, 
    width: '50px'   
    opacity: 'show',  
    fontSize: "10em",
}, 500);
```

除了定义数值，每个属性能使用’show’, ‘hide’, 和 ‘toggle’。这些快捷方式允许定制隐藏和显示动画用来控制元素的显示或隐藏


```
.animate({
    width: "toggle"
});
```

如果提供一个以+= 或 -=开始的值，那么目标值就是以这个属性的当前值加上或者减去给定的数字来计算的


```
.animate({ 
    left: '+=50px'
}, "slow");
```

- duration时间
    - 动画执行的时间，持续时间是以毫秒为单位的；值越大表示动画执行的越慢，不是越快。还可以提供’fast’ 和 ‘slow’字符串，分别表示持续时间为200 和 600毫秒。
- easing动画运动的算法
    - jQuery库中默认调用 swing。如果需要其他的动画算法，请查找相关的插件
- complete回调
    - 动画完成时执行的函数，这个可以保证当前动画确定完成后发会触发

### 13.jQuery中动画animate(下)

```
.animate( properties, options )
```

options参数

- duration - 设置动画执行的时间
- easing - 规定要使用的 easing 函数，过渡使用哪种缓动函数
- step：规定每个动画的每一步完成之后要执行的函数
- progress：每一次动画调用的时候会执行这个回调，就是一个进度的概念
- complete：动画完成回调

其中最关键的一点就是：

如果多个元素执行动画，回调将在每个匹配的元素上执行一次，不是作为整个动画执行一次


```
$('#elem').animate({
    width: 'toggle',  
    height: 'toggle'
    }, {
    duration: 5000,
    specialEasing: {
      width: 'linear',
      height: 'easeOutBounce'
    },
    complete: function() {
      $(this).after('<div>Animation complete.</div>');
    }
});
```

### 14.jQuery中停止动画stop
1. stop() 停止当前动画
1. stop(true) 停止当前执行动画元素的所有动画行为
1. stop(true,true) 停止当前执行动画元素的所有动画行为，并且直接到达动画最后一帧

### 15.jQuery中each方法的应用
用来处理对象和数组的遍历


```
jQuery.each(array, callback )
jQuery.each( object, callback )
$.each(["Aaron", "慕课网"], function(index, value) {
   //index是索引,也就是数组的索引
   //value就是数组中的值了
});

$.each(["Aaron", "慕课网"], function(index, value) {
    return false; //停止迭代
});
例子

$.each(['Aaron', '慕课网'], function(i, item) {
    $aaron.append("索引=" + i + "; 元素=" + item);
});

$.each({
    name: "张三",
    age: 18
}, function(property, value) {
    $aaron.append("属性名=" + property + "; 属性值=" + value);
});
```

### 16.jQuery中查找数组中的索引inArray
jQuery.inArray()函数用于在数组中搜索指定的值，并返回其索引值。如果数组中不存在该值，则返回 -1。，可以通过fromIndex规定查找的起始值，默认数组是0开始。


```
jQuery.inArray( value, array ,[ fromIndex ] )
```

例如：在数组中查找值是5的索引


```
$.inArray(5,[1,2,3,4,5,6,7]) //返回对应的索引：4
```

注意：

如果要判断数组中是否存在指定值，你需要通过该函数的返回值不等于(或大于)-1来进行判断

### 17.jQuery中去空格神器trim方法
jQuery.trim()函数用于去除字符串两端的空白字符

这个函数很简单，没有多余的参数用法

需要注意：

1. 移除字符串开始和结尾处的所有换行符，空格(包括连续的空格)和制表符（tab）
1. 如果这些空白字符在字符串中间时，它们将被保留，不会被移除
### 18.jQuery中DOM元素的获取get方法

```
.get( [index ] )

<a>1</a>
<a>2</a>
<a>3</a>
```

注意2点

1. get方法是获取的dom对象，也就是通过document.getElementById获取的对象
1. get方法是从0开始索引
所以第二个a元素的查找： $(a).get(1)

#### 负索引值参数

get方法还可以从后往前索引，传递一个负索引值，注意的负值的索引起始值是-1

同样是找到第二元素，可以传递 $(a).get(-2)

> get方法是获取dom对象，eq方法是获取jq对象.那么js取得的元素后面就必须跟js的方法或者属性，既$().get().style()。而不是$().get().css()。因为style是js独有的方法，而css()是jq独有的方法，get()方法后取得的js元素，它只能识别style()，而不能识别css()。

### 19.jQuery中DOM元素的获取index方法

```
.index()
.index( selector )
.index( element )
```

1. 如果不传递任何参数给 .index() 方法，则返回值就是jQuery对象中第一个元素相对于它同辈元素的位置
1. 如果在一组元素上调用 .index() ，并且参数是一个DOM元素或jQuery对象， .index() 返回值就是传入的元素相对于原先集合的位置
1. 如果参数是一个选择器， .index() 返回值就是原先元素相对于选择器匹配元素的位置。如果找不到匹配的元素，则 .index() 返回 -1
例如

```
<ul>
    <a></a>
    <li id="test1">1</li>
    <li id="test2">2</li>
    <li id="test3">3</li>
</ul>
```

$(“li”).index() 没有传递参数，反正的结果是1，它的意思是返回同辈的排列循序，第一个li之前有a元素,同辈元素是a开始为0，所以li的开始索引是1

如果要快速找到第二个li在列表中的索引,可以通过如下2种方式处理


```
$("li").index(document.getElementById("test2")) //结果：1
$("li").index($("#test2"))  //结果:1
```

