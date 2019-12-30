---
title: Vscode中使用Emmet神奇快速编写HTML代码 
date: 2018-03-25 15:15:47
categories:
- 开发
tags:
- vscode
comments: false
---

## 一、Emmet带给我们的便利
可以极大的提高代码编写的效率，不用再打<>这样的尖括号。它提供了一种非常简练的语法规则，然后立刻生成对应的 HTML 结构或者 CSS 代码，同时还有多种实用的功能帮助进行前端开发。

VsCode内置了Emmet语法,在后缀为.html/.css中输入缩写后按Tab键即会自动生成相应代码

> 请注意在VsCode新版本中按Tab不再默认启用Emmet展开缩写!需要在首选项配置中将emmet.triggerExpansionOnTab设置为true值!
> 
> 如果你的编辑器中已经有了一些html智能提示代码段,比如我的VsCode还装了HTML Snippets插件,这个与Emmet语法有部分冲突,使用Tab键时会优先使用插件的代码提示,建议禁用.

## 二、基础用法
- 元素

```
div => <div> </div> //即使用元素的名称生成标签

以下是特殊的几个
html:5 => 将生成html5标准的包含body为空基本dom
html:xt => 生成XHTML过渡文档类型,DOCTYPE为XHTML
html:4s => 生成HTML4严格文档类型,DOCTYPE为HTML 4.01
a:mail          => <a href="mailto:"\></a>
a:link          => <a href="http://"></a>
script:src      => <script src=""></script>
form:get        => <form action="" method="get"></form>input:hidden    => <input type="hidden" name=""\> input:h亦可
input:email     => <input type="email" name="" id=""\>
input:password  => <input type="password" name="" id=""\>
input:checkbox  => <input type="checkbox" name="" id=""\>
input:radio     => <input type="radio" name="" id=""\>
btn:s           => <button type="submit"\></button\>
btn:r           => <button type="reset"\></button\>
```

- 文本操作符


```html
div{这是一段文本}
<div>这是一段文本</div> //使用{文字}
```

- 属性操作符

id class

```
div.test  => <div class="test"></div>
div#pageId => <div id="pageId"></div>
```

绑定多个类名用.符号连续起来即可


```
div.test1.test2.test3
=>
<div class="test1 test2 test3"></div>
```

自定义属性使用 [attr1=’’ attr2=’’]

```
a[href='#' data-title='customer' target='_blank']
=>
<a href="#" data-title="customer" target="_blank"></a>
```

- 嵌套操作符(Nesting operators)

子级:>

```
div#pageId>ul>li 
=> 
<div id="pageId">
    <ul>
        <li></li>
    </ul>
</div>
```


同级:+

```
div#pageId+div.child
=>
<div id="pageId"></div>
<div class="child"></div>
```



- 父级:^


```
div>p.parent>span.child^ul.brother>li
=>
<div>
    <p class="parent"><span class="child"></span></p>
    <ul class="brother">
        <li></li>
    </ul>
</div>
```

- 分组操作符(Grouping)

```
div>(ul>li+span)>a
=>
<div>
<ul>
    <li></li>
    <span></span>
</ul>
<a href=""></a>
</div>
```

- 乘法

```
ul>li*3
=>
<ul>
<li></li>
<li></li>
<li></li>
</ul>
```

- 自动计数(numbering)

这个功能挺方便的对于生成重复项时增加一个序号,只需要加上$符号即可.

```
ul>li.item${item number:$}*3
<ul>
<li class="item1">item number:1</li>
<li class="item2">item number:2</li>
<li class="item3">item number:3</li>
</ul>
```

> 如果生成两位数则使用两个连续的$$,更多位数以此类推…
> 使用@修饰符，可以更改编号方向（升序或降序）和基数（例如起始值）.注意这个操作符在$之后添加
> @-表示降序,@+表示升序,默认使用升序.
> @N可以改变起始值.需要注意的是如果配合升降序使用的话N是放到+-符后.

```
ul>li.item$@-*3
=>
<ul>
<li class="item3"></li>
<li class="item2"></li>
<li class="item1"></li>
</ul>
---------------------------
ul>li.item$@-10*3
=>
<ul>
<li class="item12"></li>
<li class="item11"></li>
<li class="item10"></li>
</ul>
```

- 包装文本
听起来可能有点绕,通俗点解释就是把一段指定的文本包装成我们想要的结构.注意这个功能需要编辑器的支持,举个大栗子:
比如PM给了这样一段文本

```
首页
产品介绍
相关案例
关于我们
联系我们
而我们预期的效果是这样
<nav>
<ul>
    <li>首页</li>
    <li>产品介绍</li>
    <li>相关案例</li>
    <li>关于我们</li>
    <li>联系我们</li>
</ul>
</nav>
```

- 选中文本,按下ctrl+shift+p打开命令窗口输入ewrap
- 选择Emmet:使用缩写进行包装(Wrap with Abbreviation)选项
- 输入缩写字符nav>ul>li*按下回车键即可看到效果.
- 当然也可以在菜单=>编辑=>Emmet(M)..然后输入.

```
这里需要的注意的地方是输入的缩写代码中*所在位置不同得到的效果也是不同的.
```

另外如果给的文本带有序号的情况,我们也是可以通过缩写来处理,而不是手动删除,主要用的是|t来处理.
比如:


```
1.首页
2.产品介绍
3.相关案例
4.关于我们
5.联系我们
输入包装字符命令
nav>ul>li*|t
即可看到生成的html中自动去掉了序号
```
