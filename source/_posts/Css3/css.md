---
title:  css
date: 2019-03-07 15:32:37
categories:
- Css3
tags:
comments: false
---

### CSS选择器

id选择器(#myid)、类选择器(.myclassname)、标签选择器(div, h1, p)、相邻选择器(h1 + p)、子选择器（ul > li）、后代选择器（li a）、通配符选择器（*）、属性选择器（a[rel=”external”]）、伪类选择器（a:hover, li:nth-child）



### CSS优先级算法如何计算？

**!important > 行内样式>ID选择器 > 类选择器 > 标签 > 通配符 > 继承 > 浏览器默认属性**

*优先级就近原则，同权重情况下样式定义最近者为准;

*载入样式以最后载入的定位为准;

**优先级**

同权重:内联样式表（标签内部）>嵌入样式表（当前文件中）>外部样式表（外部文件中）。

!important >  id > class > tag

important比内联优先级高



### CSS优化、提高性能的方法有哪些？

关键选择器（key selector）。选择器的最后面的部分为关键选择器（即用来匹配目标元素的部分）；

如果规则拥有ID选择器作为其关键选择器，则不要为规则增加标签。过滤掉无关的规则（这样样式系统就不会浪费时间去匹配它们了）；

提取项目的通用公有样式，增强可复用性，按模块编写组件；增强项目的协同开发性、可维护性和可扩展性;

使用预处理工具或构建工具（gulp对css进行语法检查、自动补前缀、打包压缩、自动优雅降级）；





### CSS盒子模型了解吗，IE盒子模型会在IE的什么版本出现呢

IE5.5及更早的版本使用的是IE盒模型。IE6及其以上的版本在标准兼容模式下使用的是W3C的盒模型标准。



w3c盒子模型的范围包括margin、border、padding、content,并且content部分不包含其他部分。

当设置background颜色时候，会覆盖padding+width，当border为dashed虚线时，背景色会嵌进去



IE盒子模型的范围包括margin、border、padding、content,和w3c盒子模型不同的是，IE盒子模型的content部分包含了padding和border.

![img](http://s11.mogucdn.com/mlcdn/c45406/190611_737e5c9c9fdajfg6h8jb3k2hb3g9h_700x443.png)

box-sizing: content-box(w3c) border-box(ie)



### CSS3 新特性

**1.CSS3的选择器**

1）E:last-child 匹配父元素的最后一个子元素E。

2）E:nth-child(n)匹配父元素的第n个子元素E。 

3）E:nth-last-child(n) CSS3 匹配父元素的倒数第n个子元素E。

**2. @Font-face 特性**

**3. 圆角**

**4.阴影（Shadow）**  **5.CSS3 的渐变效果** 

background-image:-webkit-gradient(linear,0% 0%,100% 0%,from(#2A8BBE),to(#FE280E));

**6. CSS3制作特效**

1) Transition 对象变换时的过渡效果

2) Transforms 2D转换效果

主要包括 translate（水平移动）、rotate（旋转）、scale（伸缩）、skew（倾斜）

3) Animation动画特效 @keyframes



### CSS继承属性有哪些

　1、字体系列属性

　　font-family：字体系列

　　font-weight：字体的粗细

　　font-size：字体的大小

　　font-style：字体的风格

 

　2、文本系列属性

　　text-indent：文本缩进

　　text-align：文本水平对齐

　　line-height：行高

　　word-spacing：单词之间的间距

　　letter-spacing：中文或者字母之间的间距

　　text-transform：控制文本大小写（就是uppercase、lowercase、capitalize这三个）

　　color：文本颜色

 

　3、元素可见性：

　　visibility：控制元素显示隐藏

 

　4、列表布局属性：

　　list-style：列表风格，包括list-style-type、list-style-image等

 

　5、光标属性：

　　cursor：光标显示为何种形态



### 块级元素、行内元素

**块级元素**：块级大多为结构性标记

    <address>...</address>
    <center>...</center>  地址文字
    <h1>...</h1>  标题一级
    <h2>...</h2>  标题二级
    <h3>...</h3>  标题三级
    <h4>...</h4>  标题四级
    <h5>...</h5>  标题五级
    <h6>...</h6>  标题六级
    <hr>  水平分割线
    <p>...</p>  段落
    <pre>...</pre>  预格式化
    <blockquote>...</blockquote>  段落缩进   前后5个字符
    <marquee>...</marquee>  滚动文本
    <ul>...</ul>  无序列表
    <ol>...</ol>  有序列表
    <dl>...</dl>  定义列表
    <table>...</table>  表格
    <form>...</form>  表单
    <div>...</div>


**行内元素**：行内大多为描述性标记


    <span>.
    <a>...</a>  链接
    <br>  换行
    <b>...</b>  加粗
    <strong>...</strong>  加粗
    <img >  图片
    <sup>...</sup>  上标
    <sub>...</sub>  下标
    <i>...</i>  斜体
    <em>...</em>  斜体
    <del>...</del>  删除线
    <u>...</u>  下划线
    <input>...</input>  文本框
    <textarea>...</textarea>  多行文本
    <select>...</select>  下拉列表

**区别**

- 块级元素

  1.总是从新的一行开始

  2.高度、宽度都是可控的

  3.宽度没有设置时，默认为100%

  4.块级元素中可以包含块级元素和行内元素

- 行内元素

  1.和其他元素都在一行

  2.高度、宽度以及内边距都是不可控的

  3.宽高就是内容的高度，不可以改变

  4.行内元素只能行内元素，不能包含块级元素



### 一个div,position设置为absolute,再用JS设置div.style.position=""，问这时div的表现？怎么去判断？

被js覆盖，因为js会把样式写在style的属性，是行内元素，覆盖掉了。



### CSS 水平居中方式

**行内元素**
``text-align:center``



**块级元素**

定宽
``margin:0 auto；``



不定宽： 

	父级元素设置 text-align:center;
	
	子元素设置为内嵌块元素 display:inline-block;
	
	父级：relative
	
	子级：
	
	position：absolute;
	
	left:50%;
	
	magin-left: -自己宽度的一半；
	
	translate()



### CSS 垂直居中方式

    父级：relative
    
    子级：
    
    position：absolute;
    
    top:50%;
    
    magin-top: -自己宽度的一半；
    
    flex align-items:center
    
    translate()



### 说说float

float虽然已经脱离了文档流，但是他还会占据文档的位置。



### CSS实现一个旋转的动画如何实现，animation和transition有什么区别

transition主要是过渡作用，比如鼠标移入移出的动画，animation是强化版的动画。



transition 不能自行触发，通过hover等动作，或者结合js进行触发。animation可以自行运行

transition可控性相对较弱，只能够指定起始和结束的状态，而animation可以定义多个关键帧



### 听说过逐帧动画吗，如果让你实现，你会如何实现

[animation](https://www.cnblogs.com/Fengzp/p/5548493.html) @keyframes



### 前端如何实现直播的功能你了解吗，让你设计弹幕效果如何实现

后台返回一个流式媒体，前端通过video接收 



### 知道canvas吗，canvas怎么实现一个旋转的矩形

rotate()



### inline-block知道吗，应用场景都有哪些呢

结合inline和block两种属性的特性，保持行内排列的特性，也可以设置宽高。



可以应用于一切需要行内排列且要设置大小的需求，比如网页的导航栏和把a标签做按钮时。



### 浏览器最小的字体？怎么更小？

12px。使用transform-origin transform-scale



### css3和js动画哪个好

如果动画只是简单的状态切换，不需要中间过程控制，在这种情况下，css动画是优选方案。然而如果你在设计很复杂的富客户端界面或者在开发一个有着复杂UI状态的 APP。那么你应该使用js动画，这样你的动画可以保持高效，并且你的工作流也更可控。所以，在实现一些小的交互动效的时候，就多考虑考虑CSS动画。对于一些复杂控制的动画，使用javascript比较可靠。



#### JS动画

缺点：(1)JavaScript在浏览器的主线程中运行，而主线程中还有其它需要运行的JavaScript脚本、样式计算、布局、绘制任务等,对其干扰导致线程可能出现阻塞，从而造成丢帧的情况。

​        (2)代码的复杂度高于CSS动画

优点：(1)JavaScript动画控制能力很强, 可以在动画播放过程中对动画进行控制：开始、暂停、回放、终止、取消都是可以做到的。

​        (2)动画效果比css3动画丰富,有些动画效果，比如曲线运动,冲击闪烁,视差滚动效果，只有JavaScript动画才能完成

​        (3)CSS3有兼容性问题，而JS大多时候没有兼容性问题



#### CSS动画

缺点：

   (1)运行过程控制较弱,无法附加事件绑定回调函数。CSS动画只能暂停,不能在动画中寻找一个特定的时间点，不能在半路反转动画，不能变换时间尺度，不能在特定的位置添加回调函数或是绑定回放事件,无进度报告

   (2)代码冗长。想用 CSS 实现稍微复杂一点动画,最后CSS代码都会变得非常笨重。



优点： (1)浏览器可以对动画进行优化。

-  浏览器使用与 requestAnimationFrame 类似的机制，requestAnimationFrame比起setTimeout，setInterval设置动画的优势主要是:1)requestAnimationFrame 会把每一帧中的所有DOM操作集中起来，在一次重绘或回流中就完成,并且重绘或回流的时间间隔紧紧跟随浏览器的刷新频率,一般来说,这个频率为每秒60帧。2)在隐藏或不可见的元素中requestAnimationFrame不会进行重绘或回流，这当然就意味着更少的的cpu，gpu和内存使用量。
- 强制使用硬件加速 （通过 GPU 来提高动画性能）



总结：

1、css用于简单的动画，比如过渡；js用于复杂的动画，比如需要对动画进行开始，暂停，回放灯，css不能暂停。

2、css有兼容性问题，但是他可以gpu加速，提高性能



同样的动画，js能实现，css也能实现，选择哪个呢

1、如果有兼容性，选择Js

2、但是js会损耗浏览器性能，单线程的，处理动画一个接一个，会卡顿



### 显示/隐藏

display:block/none

visibility:visible/hidden



区别：

none 隐藏内容，会脱离文本流，不占据页面空间 引发重绘 回流

hidden 隐藏内容，不会脱离文本流，会继续占据页面空间 引发重绘



### @media

all screen print(用于打印机和打印预览) speech(用于屏幕阅读器等发声设备)



### px rem em

1、px 像素，是一个固定大小的单元

2、rem css3新增，相对长度单位，相对于html元素

3、em 相对长度单位，不是固定的，它相对于当前对象内文本的字体尺寸，会继承父级元素，如果没设置，相对于浏览器的默认字体尺寸16px



### 网页上那个小图标是通过什么设置的

	<link rel="SHORTCUT ICON" href="ico图标文件地址">



### meta里有哪些属性

两个属性：name http-equiv



**1、name**

name属性主要用于描述网页，比如网页的关键词，叙述等。对应的content，是对name的具体描述，便于搜索引擎抓取

	<meta name="参数" content="具体的描述">



参数

    <meta name="keywords" content="Lxxyx,博客，文科生，前端">
    <meta name="description" content="文科生，热爱前端与编程。目前大二，这是我的前端博客">
    <meta name="viewport" content="width=device-width, initial-scale=1"> //移动端的窗口
    <meta name="robots" content="none">
    <meta name="author" content="Lxxyx,841380530@qq.com">
    <meta name="generator" content="Sublime Text3">
    <meta name="copyright" content="Lxxyx"> //代表该网站为Lxxyx个人版权所有。
    <meta name="revisit-after" content="7 days" > //搜索引擎爬虫重访时间
    <meta name="renderer" content="webkit"> //默认webkit内核
    <meta name="renderer" content="ie-comp"> //默认IE兼容模式
    <meta name="renderer" content="ie-stand"> //默认IE标准模式



**2、http-equiv**

	<meta http-equiv="参数" content="具体的描述">



参数


    <meta http-equiv="content-Type" content="text/html;charset=utf-8">  //旧的HTML，不推荐 //设定网页字符集
    <meta charset="utf-8"> //HTML5设定网页字符集的方式，推荐使用UTF-8
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1"/> //指定IE和Chrome使用最新版本渲染当前页面
    <meta http-equiv="cache-control" content="no-cache">
    <meta http-equiv="expires" content="Sunday 26 October 2016 01:00 GMT" />
    <meta http-equiv="refresh" content="2；URL=http://www.lxxyx.win/"> //意思是2秒后跳转向我的博客
    <meta http-equiv="Set-Cookie" content="name, date"> //格式
    
    <meta http-equiv="Set-Cookie" content="User=Lxxyx; path=/; expires=Sunday, 10-Jan-16 10:00:00 GMT"> //具体范例