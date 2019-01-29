---
title: vue过渡动画
date: 2017-09-09 15:15:47
categories:
- 前端技术篇
tags:
- VUE
- 饿了么
comments: false
---

在vue中，实现过渡动画一般是下面这样:

```
<transition name="fade">
    <div></div>
</transition>
```

用一个transition对元素或者组件进行封装.
在过渡的时候，会有 4 个(CSS)类名在 enter/leave 的过渡中切换。

- v-enter: 定义进入过渡的开始状态。在元素被插入时生效，在下一个帧移除。
- v-enter-active: 定义进入过渡的结束状态。在元素被插入时生效，在 transition/animation 完成之后移除。
- v-leave: 定义离开过渡的开始状态。在离开过渡被触发时生效，在下一个帧移除。
- v-leave-active: 定义离开过渡的结束状态。在离开过渡被触发时生效，在 transition/animation 完成之后移除。

上面是官方文档的原话，但是这4个类名如何运用呢？下面看个例子


```
<transition name="fold">
    <div v-show="show" class="example"></div>
</transition>

css:
.example {
    width: 100px;
    height: 100px;
    transform: translate3d(0, -100px, 0);
}
.fold-enter-active, .fold-leave-active {
    transition: all .5s;
}
.fold-enter, .fold-leave-active {
    transform: translate3d(0, 0, 0);
}
```

当show = true时:

这里的fold-enter，是在元素显示的一瞬间被加入，然后瞬间被移除，在上面的例子中，example这个块状元素本来因为有了transform: translate3d(0, -100px, 0)这个样式，所以被上移了100px,但是因为fold-enter，所以将example这个块状元素移到了原位，但是fold-enter因为瞬间就被移除掉了，所以这时候的元素的样式又变成了transform: translate3d(0, -100px, 0),但是这时候fold-enter-active这个样式就生效了，因为设置了transition: all .5s,所以元素会在0.5s的时间内上移100px,在过渡动画结束之后,fold-enter-active这个类马上被移除.

当show = false时:

example不会马上消失，这个时候fold-leave-active这个类就起作用了,example这时的位置是在-100px，fold-leave-active定义的是过渡的结束状态：transform: translate3d(0, 0, 0);，但是同时又设置了transition: all .5s，所以这时元素会在0.5s的时间内下移100px.之后fold-leave-active被移除.

另外，还可以在动画中调用js钩子函数:


```
<transition
  v-on:before-enter="beforeEnter"
  v-on:enter="enter"
  v-on:after-enter="afterEnter"
  v-on:enter-cancelled="enterCancelled"
  v-on:before-leave="beforeLeave"
  v-on:leave="leave"
  v-on:after-leave="afterLeave"
  v-on:leave-cancelled="leaveCancelled"
>
</transition>
```
