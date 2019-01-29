---
title: stylus
date: 2017-08-18 15:15:47
categories:
- 前端技术篇
tags:
- css预处理器
comments: false
---

### 一、选择器
(1).使用缩排和凹排代替花括号{以及}

```
body
  color white

body {
  color: #fff;
}

body
  color: white
```

(2).允许你使用逗号为多个选择器同时定义属性

```
textarea, input
  border 1px solid #eee

textarea,
input {
  border: 1px solid #eee;
}
```

唯一的例外就是长得像属性的选择器。例如，下面的foo bar baz可能是个属性或者是选择器。


```
foo bar baz
> input
  border 1px solid
```

为解决这个原因，我们可以在尾部加个逗号：


```
foo bar baz,
form input,
> a
  border 1px solid
```

(3).字符&指向父选择器。

```
textarea
input
  color #A7A7A7
  &:hover
    color #000

textarea,
input {
  color: #a7a7a7;
}
textarea:hover,
input:hover {
  color: #000;
}
```

(4).unquote() 处理Stylus无法处理的属性值

```
filter unquote('progid:DXImageTransform.Microsoft.BasicImage(rotation=1)')

filter progid:DXImageTransform.Microsoft.BasicImage(rotation=1)
```

### 二、变量
(1).可以指定表达式为变量，然后在我们的样式中贯穿使用

```
font-size = 14px
body
  font font-size Arial, sans-seri

body {
  font: 14px Arial, sans-serif;
}
```

(2).变量甚至可以组成一个表达式列表

```
font-size = 14px
font = font-size "Lucida Grande", Arial
body
  font font sans-serif

body {
  font: 14px "Lucida Grande", Arial sans-serif;
}
```

(3).不需要分配值给变量就可以定义引用属性。

```
#logo
  position: absolute
  top: 50%
  left: 50%
  width: 150px
  height: 80px
  margin-left: -(@width / 2)
  margin-top: -(@height / 2)
```

### 三、插值
通过使用{}字符包围表达式来插入值，其会变成标识符的一部分。


```
table
  for row in 1 2 3 4 5
    tr:nth-child({row})
      height: 10px * row

table tr:nth-child(1) {
  height: 10px;
}
table tr:nth-child(2) {
  height: 20px;
}
table tr:nth-child(3) {
  height: 30px;
}
table tr:nth-child(4) {
  height: 40px;
}
table tr:nth-child(5) {
  height: 50px;
}
```

### 四、运算符
当在属性值内使用/时候，你必须用括号包住。

```
font: (14px/1.5);
```

### 五、Mixins 混合书写
混入和函数定义方法一致，但是应用却大相径庭。

> 下面有定义的border-radius(n)方法，其却作为一个mixin（如，作为状态调用，而非表达式）调用。
> 
> 当border-radius()选择器中调用时候，属性会被扩展并复制在选择器中。


```
border-radius(n)
  -webkit-border-radius n
  -moz-border-radius n
  border-radius 
form input[type=button]
  border-radius(5px)

编译成：
form input[type=button] {
  -webkit-border-radius: 5px;
  -moz-border-radius: 5px;
  border-radius: 5px;
}
```

注意到我们混合书写中的border-radius当作了属性，而不是一个递归函数调用


```
border-radius()
  -webkit-border-radius arguments
  -moz-border-radius arguments
  border-radius arguments
```

### 六、方法
(1).返回值

```
很简单的例子，两数值相加的方法：

add(a, b)
  a + b
我们可以在特定条件下使用该方法，如在属性值中：

body 
  padding add(10px, 5)

渲染：
body {
  padding: 15px;
}
```

(2).默认参数
可选参数往往有个默认的给定表达。在Stylus中，我们甚至可以超越默认参数。


```
例如：
add(a, b = a)
  a + b

add(10, 5)
// => 15

add(10)
// => 20

注意：因为参数默认是赋值，我们可可以使用函数调用作为默认值。

add(a, b = unit(a, px))
  a + b
```

(3).函数体
通过内置unit()把单位都变成px, 因为赋值在每个参数上，因此，我们可以无视单位换算。


```
add(a, b = a)
  a = unit(a, px)
  b = unit(b, px)
  a + b

add(15%, 10deg)
// => 25
```

(4).变量函数
我们可以把函数当作变量传递到新的函数中。例如，invoke()接受函数作为参数，因此，我们可以传递add()以及sub().


```
invoke(a, b, fn)
  fn(a, b)

add(a, b)
  a + b

body
  padding invoke(5, 10, add)
  padding invoke(5, 10, sub)

结果：
body {
  padding: 15;
  padding: -5;
}
```

(5).参数

```
arguments是所有函数体都有的局部变量，包含传递的所有参数。

例如：
sum()
  n = 0
  for num in arguments
    n = n + num

sum(1,2,3,4,5)
// => 15
```

### 七、关键字
Stylus支持关键字参数，或”kwargs”. 允许你根据相关参数名引用参数。


```
body {
  color: rgba(255, 200, 100, 0.5);
  color: rgba(red: 255, green: 200, blue: 100, alpha: 0.5);
  color: rgba(alpha: 0.5, blue: 100, red: 255, 200);
  color: rgba(alpha: 0.5, blue: 100, 255, 200);
}
等同于：
body {
   color: rgba(255,200,100,0.5);
   color: rgba(255,200,100,0.5);
   color: rgba(255,200,100,0.5);
   color: rgba(255,200,100,0.5);
}
```

查看函数或混合书写中接受的参数，可以使用p()方法。


```
p(rgba)
生成：
inspect: rgba(red, green, blue, alpha)
```

### 八、内置方法
Stylus支持name…形式的其余参数。这些参数可以消化传递给混写或函数的参数们。这在处理浏览器私有属性，如-moz或-webkit的时候很管用。


```
box-shadow()
  -webkit-box-shadow arguments
  -moz-box-shadow arguments
  box-shadow arguments

body
  box-shadow #ddd 1px 1px, #eee 2px 2px

body {
  -webkit-box-shadow: #ddd 1px 1px, #eee 2px 2px;
  -moz-box-shadow: #ddd 1px 1px, #eee 2px 2px;
  box-shadow: #ddd 1px 1px, #eee 2px 2px;
}
```

### 九、注释
Stylus支持三种注释，单行注释，多行注释，以及多行缓冲注释。

(1).单行注释
跟JavaScript一样，双斜杠，CSS中不输出。


```
// 我是注释!
body
  padding 5px // 蛋疼的padding
```

(2).多行注释
多行注释看起来有点像CSS的常规注释。然而，它们只有在compress选项未启用的时候才会被输出。


```
/*
 * 给定数值合体
 */

add(a, b)
      a + b
```

(3).多行缓冲注释
跟多行注释类似，不同之处在于开始的时候，这里是/*!. 这个相当于告诉Stylus压缩的时候这段无视直接输出。


```
/*!
 * 给定数值合体
 */

add(a, b)
  a + b
```

### 十、条件
### 十一、迭代
### 十二、@import
### 十三、@media

```
@media print
  #header
  #footer
    display none

生成为：
@media print {
  #header,
  #footer {
    display: none;
  }
}
```

### 十四、@font-face

```
@font-face
  font-family Geo
  font-style normal
  src url(fonts/geo_sans_light/GensansLight.ttf)

.ingeo
  font-family Geo
生成为：
@font-face {
  font-family: Geo;
  font-style: normal;
  src: url("fonts/geo_sans_light/GensansLight.ttf");
}
.ingeo {
  font-family: Geo;
}
```

### 十五、@keyframes

```
@keyframes pulse
0%
  background-color red
  opacity 1.0
  -webkit-transform scale(1.0) rotate(0deg)
33%
  background-color blue
  opacity 0.75
  -webkit-transform scale(1.1) rotate(-5deg)
67%
  background-color green
  opacity 0.5
  -webkit-transform scale(1.1) rotate(5deg)
100%
  background-color red
  opacity 1.0
  -webkit-transform scale(1.0) rotate(0deg)
生成为：
@-webkit-keyframes pulse {
  0% {
  background-color: red;
  opacity: 1;
    -webkit-transform: scale(1) rotate(0deg);
  }

  33% {
    background-color: blue;
    opacity: 0.75;
    -webkit-transform: scale(1.1) rotate(-5deg);
  }

  67% {
    background-color: green;
    opacity: 0.5;
    -webkit-transform: scale(1.1) rotate(5deg);
  }

  100% {
    background-color: red;
    opacity: 1;
    -webkit-transform: scale(1) rotate(0deg);
  }

}
```

### 十六、@extend

```
message,
.warning {
  padding: 10px;
  border: 1px solid #eee;
}

.warning {
  color: #E2E21E;
}

.message {
  padding: 10px;
  border: 1px solid #eee;
}

.warning {
  @extend .message;
  color: #E2E21E;
}
```

### 十七、url()
### 十八、字面量css
如果遇到Stylus搞不定的特殊需求，你可以使用@css使其作为CSS字面量解决之。


```
@css {
  body {
    font: 14px;
  }
}
编译为：
body {
  font: 14px;
}
```

### 十九、CSS样式解析
支持css,stylus

### 二十、使用

```
#npm -g stylus
```


在.styl文件夹中 stylus <demo.styl> demo.css

```
stylus -w demo.styl -o demo.css
```

