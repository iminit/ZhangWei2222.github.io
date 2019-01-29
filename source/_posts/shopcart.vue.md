---
title: shopcart.vue
date: 2017-09-09 15:15:47
categories:
- 前端技术篇
tags:
- VUE
- 饿了么
comments: false
---


html代码

```
<template>
<!--vue的render渲染需要有一个root的根dom层,所以创建一个单独的div来包裹-->
  <div>
  <!--这是shopcart的主代码-->
    <div class="shopcart">
    <!--用toggleList点击显示隐藏购物车列表-->
      <div class="content" @click="toggleList">
        <div class="content-left">
          <div class="logo-wrapper">
            <div class="logo" :class="{'highlight':totalCount>0}">
              <i class="icon-shopping_cart" :class="{'highlight':totalCount>0}"></i>
            </div>
            <div class="num" v-show="totalCount>0">{{totalCount}}</div>
          </div>
          <div class="price" :class="{'highlight':totalPrice>0}">￥{{totalPrice}}</div>
          <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
        </div>
        <!--支付-->
        <div class="content-right" @click.stop.prevent="pay">
        <!--这里class绑定的是一个payClass计算属性-->
          <div class="pay" :class="payClass">
            {{payDesc}}
          </div>
        </div>
      </div>
      <!--省略了ball-container是动画小球的部分-->
      <!--shopcart的列表详情部分-->
      <transition name="fold">
        <div class="shopcart-list" v-show="listShow">
          <div class="list-header">
            <h1 class="title">购物车</h1>
            <span class="empty" @click="empty">清空</span>
          </div>
          <div class="list-content" ref="listContent">
            <ul>
              <li class="food" v-for="food in selectFoods">
                <span class="name">{{food.name}}</span>
                <div class="price">
                  <span>￥{{food.price * food.count}}</span>
                </div>
                <!--列表里面可以进行操作,所以会复用一个cartcontrol的组件-->
                <div class="cartcontrol-wrapper">
                  <cartcontrol @add="addFood" :food="food"></cartcontrol>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </transition>
    </div>
    <!--打开shopcart列表的时候背景需要虚化并且动画-->
    <transition name="fade">
      <div class="list-mask" @click="hideList" v-show="listShow"></div>
    </transition>
  </div>
</template>
```

js代码


```
import BScroll from 'better-scroll';
import cartcontrol from '../../components/cartcontrol/cartcontrol';

export default{
  props: {
    selectFoods: {
      type: Array,
      default(){ //data必须是一个函数,这是默认值data的写法
        return [ 
          {}
        ];
      }
    },
    deliveryPrice: { 
      type: Number,
      default: 0
    },
    minPrice: {
      type: Number,
      default: 0
    }
  },
  data(){
    return {
      fold: true //通过这个变量来控制隐藏和显示购物车列表,true代表隐藏,折叠起来
    }
  },
  computed: {
    totalPrice(){ //计算总价
      let total = 0;
      this.selectFoods.forEach((food) => {
        total += food.price * food.count; //这个count就是在goods组件里面添加的
      })
      return total;
    },
    totalCount(){ //计算总数量
      let count = 0;
      this.selectFoods.forEach((food) => {
        count += food.count;
      })
      return count;
    },
    payDesc(){ //只是简单的做一些支付显示的计算
      if (this.totalPrice === 0) {
        return `¥${this.minPrice}元起送`;
      } else if (this.totalPrice < this.minPrice) {
        let diff = this.minPrice - this.totalPrice;
        return `还差¥${diff}元配送`;
      } else {
        return `去结算`;
      }
    },
    payClass(){
      if (this.totalPrice < this.minPrice) {
        return 'not-enough';
      } else {
        return 'enough';
      }
    },
    listShow(){ //自动计算购物车列表是否显示和隐藏
      if (!this.totalCount) { //自动判断商品数量为空
        this.fold = true; //直接隐藏
        return false;
      }
      let show = !this.fold; //当flod为false的时候显示购物车列表
      if (show) { 
        this.$nextTick(() => { //异步执行滚动列表初始化
          if (!this.scroll) {
            this.scroll = new BScroll(this.$refs.listContent, {
              click: true
            });
          } else {
            this.scroll.refresh();
          }
        });
      }
      return show;
    }
  },
  methods: {
    toggleList(){
      if (!this.totalCount) {
        return;
      }
      this.fold = !this.fold;
    },
    empty() { //通过清空数据属性来控制购物车清空
      this.selectFoods.forEach((food) => {
        food.count = 0;
      });
    },
    hideList() {
      this.fold = true;
    },
    pay() {
      if (this.totalPrice < this.minPrice) {
        return;
      }
      window.alert(`支付${this.totalPrice}元`);
    }
  },
  components: {
    cartcontrol
  }
}
```

- 购物车列表的显示和隐藏(折叠)是通过数据fold来决定的,他是通过计算属性listshow来实现的,这样达到了不用操作dom就可以改变dom行为的效果,vue的数据驱动设计.
- 清空按钮也是这样实现的,通过批量设置count属性来实现
- 抛物线小球部分参考:关于购物车添加按钮的动画
- 因为打开shopcart详情页会出现两个内容:
    -shopcart-list部分参考:八.shopcart-list购物车详情页
    - cartcontrol部分参考:cartcontrol.vue购物车操作按钮
    - 又因为这个cartcontrol是有几个地方都需要用到的,所以将其组件化处理
css代码

```
@import "../../common/stylus/mixin.styl"

  .shopcart
    position: fixed //这是固定在页面底部的购物车栏目
    left: 0
    bottom: 0
    z-index: 50 
    width: 100%
    height: 48px
    .content
      display: flex //flex布局
      background: #141d27
      font-size: 0
      .content-left
        flex: 1
        .logo-wrapper
          display: inline-block
          position: relative
          top: -10px //因为要凸出购物车栏目一点点,设计需要
          margin: 0 12px
          padding: 6px
          width: 56px //因为使用了border-box的关系,可以直接写设计的尺寸
          height: 56px
          box-sizing: border-box //border-box可以省去计算border和内边距
          vertical-align: top
          border-radius: 50%
          background: #141d27
          .logo
            width: 100%
            height: 100%
            border-radius: 50%
            text-align: center
            background: #2b343c
            &.highlight
              background: rgb(0, 160, 220)
            .icon-shopping_cart
              line-height: 44px
              font-size: 24px
              color: #80858a
              &.highlight
                color: #ffffff
          .num
            position: absolute
            top: 0
            right: 0
            width: 24px
            height: 16px
            line-height: 16px
            text-align: center
            border-radius: 16px
            font-size: 9px
            color: #ffffff
            background: rgb(240, 20, 20)
            box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.4)
        .price
          display: inline-block
          vertical-align: top
          margin-top: 12px
          line-height: 24px
          padding-right: 12px
          box-sizing: border-box
          border-right: 1px solid rgba(255, 255, 255, 0.1)
          font-size: 16px
          font-weight: 700
          color: rgba(255, 255, 255, 0.4)
          &.highlight
            color: #ffffff
        .desc
          display: inline-block
          vertical-align: top
          margin: 12px 0 0 12px
          line-height: 24px
          color: rgba(255, 255, 255, 0.4)
          font-size: 10px
      .content-right
        flex: 0 0 105px //flex布局固定右边空间
        width: 105px
        .pay
          height: 48px
          line-height: 48px
          text-align: center
          font-size: 12px
          color: rgba(255, 255, 255, 0.4)
          font-weight: 700
          &.not-enough
            background: #2b333b
          &.enough
            background: #00b43c
            color: #ffffff
//省略了小球动画的css
```



这里主要注意border-box的使用,举例logo-wrapper
- 宽高都是56px,并且需要实现内边距是6px


- 不用border-box的话,就要计算图片的区域是多大,然后加上6px内边距等于56px


- 用了border-box的话,就不需要计算图片区域了,直接写56px,即使加上6px内边距也不撑大56px的区域,只会自动缩小内部图片区