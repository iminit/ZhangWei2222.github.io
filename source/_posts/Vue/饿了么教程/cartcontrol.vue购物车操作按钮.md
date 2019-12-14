---
title: cartcontrol.vue购物车操作按钮
date: 2017-09-09 15:15:47
categories:
- Vue
tags:
- 饿了么教程
comments: false
---

html代码
知识点:

- 使用了vue2的动画实现方式-transition,这里主要在css上设计来配合vue2
- cartcontrol这个模块主要通过三个小模块实现,删除按钮,显示数量块,增加按钮
- 删除按钮和增加按钮都带有一个click方法,并且都有v-show来根据food的数量来控制显示,而显示数量块就只是单纯显示food的数量

js代码
知识点:

- 引入vue模块,因为需要使用Vue.set方法
- 接收来自goods.vue的food数据,然后使用
- 有2个方法,一个是addCart,一个是decreaseCart,分别对应html代码中的@click
- addCart方法,默认会传入event原生dom事件
    - 鉴于bsscrol插件的原因,需要return掉非bscrol的事件
    - 需要注意的是这里的food并没有count属性,所需要手动设置,虽然之前goods会将有count属性的food做自动处理,但是最开始的时候,所有food都没有count属性,并且cartcontrol是最开始触发对food的count属性进行添加的(点击一次就会对count++,增加一个food)
    - food数据是从父组件goods.vue传入的,所以对这个数据的修改,也能够反应到父组件,而这里使用Vue.set直接对vue的data进行对象数据写入,对food数据添加了一个count的属性,用来控制每一个food的被选中的数量,也因为shopcart.vue的数据也是从父组件goods.vue传入的,使用同一个food数据,从而关联到[shopcart.vue购物车]()的food的购买数量统计
- decreaseCart方法,同上


```
<script>
  import Vue from 'vue';

  export default{
    props: {
      food: {
        type: Object
      }
    },
    methods: {
      addCart(event){
        if (!event._constructed) {
          return false;
        }
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1); //遇到没有这个属性的,会强行添加一个
        } else {
          this.food.count++; 
        }
        this.$emit('add', event.target);
      },
      decreaseCart(event){
        if (!event._constructed) {
          return false;
        }
        if (this.food.count) {
          this.food.count--;
        }
      }
    }
  };
</script>
```

css代码
知识点:


- vue2的动画
- Vue 提供了 transition 的封装组件,就是之前的
- 这个动画是一个滚动淡入和淡出的效果
- 首先,定义了这个动画效果的名字为move(),并且将需要这个效果的内容用一个dom的div包裹起来,这个效果分2层,外层和内层inner(外层和内层用class来区分,通过增加一个内层class inner来区分)
- 然后,设置原始的动画状态(外层和内层)
    - 外层设置了透明度为1和transform变形的3d位置
    - 内层设置了滚动角度和淡入淡出效果
- 然后,设置vue的动画过渡属性
    - move-enter-active和move-leave-active
        - 这里设置匀速过渡效果是为了在动画进入过渡的结束状态和动画离开过渡的结束状态都是匀速过渡的变化效果,至于变化成什么效果需要看move-enter和move-leave-active
    - move-enter和move-leave-active
        - 这里设置外层的透明度为0(内层会继承)和变形的3d位置

        - 这里设置了内层滚动角度
        - 这么做实现了动画在进入过渡后和离开过渡后都会内外层,并且会出现内层一边滚动到指定位置然后消失的效果
- font-size的使用
- 因为使用行内元素,所以需要通过设置font-size为0来取消间隙
- 对于一些设计上的图标过小(操作区域太小)影响用户操作体验的情况,需要增加一个padding来实现增大操作区域的目的

```
<style lang="stylus" rel="stylesheet/stylus">
  .cartcontrol
    font-size: 0
    .cart-decrease //外层动画原始状态
      display: inline-block
      padding: 6px
      opacity: 1 
      transform: translate3d(0, 0, 0) 
      .inner //内层动画原始状态
        display: inline-block
        line-height: 24px
        font-size: 24px
        color: rgb(0, 160, 220)
        transition: all 0.4s linear 
        transform: rotate(0)
      &.move-enter-active, &.move-leave-active //vue动画过渡属性     
        transition: all 0.4s linear
      &.move-enter, &.move-leave-active //vue动画过渡属性
        opacity: 0
        transform: translate3d(24px, 0, 0)
        .inner
          transform: rotate(180deg)
    .cart-count
      display: inline-block
      vertical-align: top
      width: 12px
      padding-top: 6px
      line-height: 24px
      text-align: center
      font-size: 10px
      color: rgb(147, 153, 159)
    .cart-add
      display: inline-block
      padding: 6px
      line-height: 24px
      font-size: 24px
      color: rgb(0, 160, 220)
</style>
```
