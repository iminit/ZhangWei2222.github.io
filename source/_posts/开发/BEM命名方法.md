---
title: BEM命名方法
date: 2018-03-25 15:15:47
categories:
- 开发
tags:
- 命名
comments: false
---


## 1.为什么选择这种方法命名呢？
以往命名的方法是

```html
html结构
<div class="main">
    <div class="bigBox">
        <img src="" alt="">
        <div class="text">
            <span></span>
            <h></h>
        </div>
    </div>
</div>

stylus结构
.main
    .bigBox
        img
        .text
            span
            h

css结构
.main{}
.main .bigBox{}
.main .bigBox img{}
.main .bigBox .text{}
.main .bigBox .text span{}
.main .bigBox .text h{}
```

如果某一天，不需要.bigBox这个层，那么整个stylus css都要改，因为所有都有.bigBox这个类前提

为更有效率的打码，更有逻辑的打码，现在有一种命名方法，BEM，即block element modifier

## 2.BEM命名规则
block element modifier
块 元素 修饰符

块与元素用__连接，元素和修饰符用.连接

对一个组件（拿到设计稿，需要看清哪些可以做成一个个组件），可以这样命名

```html
<div class="link-btn">
    <span class="link-btn__main-title red"></span>
    <span class="link-btn__sub-title"></span>
</div>
```

这样在stylus中就可以这样

```stylus
.link-btn
  &__main-title
    &.red
    &__sub-title
```

编译出来的css就是

```css
.link-btn{}
.link-btn__main-title{}
.link-btn__main-title red{}
.link-btn__sub-title
```

