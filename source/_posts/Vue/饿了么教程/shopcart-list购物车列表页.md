---
title: shopcart-list购物车列表页
date: 2017-09-09 15:15:47
categories:
- Vue
tags:
- 饿了么教程
comments: false
---

- 需要使用better-scroll进行列表内容滚动绑定
- 使用vue2的transition动画过渡处理动画
- 使用子组件cartcontrol处理购物车的添加删除操作cartcontrol.vue购物车操作按钮


```
<!--transition是vue2的动画格式-->
<transition name="fold"> 
    <div class="shopcart-list" v-show="listShow">
        <div class="list-header"></div>
        <div class="list-content" ref="listContent"></div>
    </div>
</transition>
```

### 购物车详情内容shopcart-list
一共有两块,list-header,list-content,这是根据设计稿内容确定的

#### list-header
绑定清空按钮


```
<div class="list-header">
            <h1 class="title">购物车</h1>
            <!--绑定empty方法清空购物车内容-->
            <span class="empty" @click="empty">清空</span>
</div>
```

通过设置selectFoods里面所有数据的count为0来实现清空的目的


```
empty() { //清空购物车内容
    this.selectFoods.forEach((food) => {
      food.count = 0;
    });
},
```

#### list-content
这里也分开两部分来说:

第一部分:

- 使用ref来绑定一个dom,然后被vue使用,ref=”listContent”,这是为了将list-content的dom被使用,然后实例化better-scroll实现滚动list的效果
- 购物车的清单是通过遍历selectFoods数组(通过父组件goods处理并传入goods.vue)得到的,v-for=”food in selectFoods”
- shopcart-list本身也有一个渐入渐出的动画
- list-content本身没有高度,是靠header和content撑开

第二部分:

- 复用子组件cartcontrol,因为这个子组件在购物车内容和购物车详情页的列表里都有使用,所以单独独立出来,复用代码
- 子组件cartcontrol会有2个动画,一个动画是他自己本身的水平滚动,一个是动画是要跟shopcart关联的抛物线滚动cartcontrol.vue购物车操作按钮
- 代码部分:
html代码


```
<!--ref是用来给better-scroll绑定的dom-->
<div class="list-content" ref="listContent">
  <ul>
  <!--遍历selectFoods获得被勾选的所有数据-->
    <li class="food" v-for="food in selectFoods">
      <span class="name">{{food.name}}</span>
      <div class="price">
        <span>￥{{food.price * food.count}}</span>
      </div>
      <!--购物车详情列表里面的购物车按钮组件cartcontrol-->
      <div class="cartcontrol-wrapper">
        <cartcontrol @add="addFood" :food="food"></cartcontrol>
      </div>
    </li>
  </ul>
</div>
```

js代码

```
import BScroll from 'better-scroll'; //导入better-scroll
import cartcontrol from '../../components/cartcontrol/cartcontrol'; //导入cartcontrol组件

export default{ //导出到当前vue文件内使用
  props: { //父组件goods传入数据
    selectFoods: { //被勾选的所有foods的数据
      type: Array, //默认值类型
      default(){ //默认值,并且vue的data数据是一个函数写法实现的
        return [
          {}
        ];
      }
    },
  },
  data(){
    return {
      fold: true // 控制购物车折叠效果的标志,默认是true代表折叠
    }
  },
  computed: {
    totalCount(){ //在这里被用来判断购物车是否有东西
      let count = 0;
      this.selectFoods.forEach((food) => {
        count += food.count;
      })
      return count;
    },
    listShow(){ //购物车是否展示内部列表处理
      if (!this.totalCount) { //判断没有数量的情况
        this.fold = true; //折叠属性设置为true,即折叠起来,主要是更新fold的状态
        return false; //返回false是给v-show使用,false会display none
      }
      let show = !this.fold; //使用fold的状态取反来实现常用的切换状态处理逻辑
       if (show) { //展示购物车列表内容
        this.$nextTick(() => { //异步处理,提高体验,防止卡顿
          if (!this.scroll) { //如果没有绑定过better-scroll就绑定一个
          //用$refs来绑定vue的dom元素
            this.scroll = new BScroll(this.$refs.listContent, {
              click: true //让better-scroll将click事件也传递
            });
          } else { //使用better-scroll的内置事件refresh直接更新绑定,不用重新实例化一次better-scroll
            this.scroll.refresh();
          }
        });
      }
      return show; //返回布尔值给v-show使用
    }
  },
  methods: {
    toggleList(){ //封装一个方法来控制购物车列表的显示
      if (!this.totalCount) {
        return;
      }
      this.fold = !this.fold; //切换状态的处理
    },
  },
  components: {
    cartcontrol //注册cartcontrol组件,以便当前vue文件能够调用
  }
```

css代码


```
@import "../../common/stylus/mixin.styl"

   .shopcart-list
      position: absolute //因为需要固定在app主界面的底部(非footer)
      left: 0
      top: 0
      z-index: -1 //因为要避免遮挡footer的图标
      width: 100%
      transform: translate3d(0, -100%, 0) //设置css方式的vue动画初始值,纵坐标的-100%是指相反方向的当前元素的100%高度,这是"入"
      &.fold-enter-active, &.fold-leave-active //shopcart-list的渐入渐出动画,这是"渐"
        transition: all 0.5s //全部元素0.5秒过渡变化
      &.fold-enter, &.fold-leave-active // 这是"出"
        transform: translate3d(0, 0, 0) //3d变形
      .list-header
        height: 40px 
        line-height: 40px
        padding: 0 18px
        background: #f3f5f7
        border-bottom: 1px solid rgba(7, 17, 27, 0.1)
        .title
          float: left
          font-size: 14px
          color: rgb(7, 17, 27)
        .empty
          float: right
          font-size: 12px
          color: rgb(0, 160, 220)

      .list-content
        padding: 0 18px
        max-height: 217px //因为不固定高度,所以用max-height
        overflow: hidden //要实现滚动,就要隐藏超出部分
        background: #fff
        .food
          position: relative
          padding: 12px 0
          box-sizing: border-box
          border-1px(rgba(7, 17, 27, 0.1))//要处理1px边框问题,引入mixin.styl
          .name
            line-height: 24px
            font-size: 14px
            color: rgb(7, 17, 27)
          .price
            position: absolute
            right: 90px
            bottom: 12px
            line-height: 24px
            font-size: 14px
            font-weight: 700
            color: rgb(240, 20, 20)
          .cartcontrol-wrapper
            position: absolute
            right: 0
            bottom: 6px
```

## 购物车遮罩list-mask

```
<!--购物车详情页的遮罩并且动画处理-->
<transition name="fade">
  <div class="list-mask" @click="hideList" v-show="listShow"></div>
</transition>
hideList() { //遮罩隐藏
        this.fold = true;
      },
```

- 遮罩的动画是普通的渐隐渐现,使用css3实现
- 只需要操作数据属性就能够实现
- 遮罩的层级是跟shopcart-list同级的,是因为同一层
- 遮罩触发会放到shopcart组件的content元素,使用的方法是toggleList


```
.list-mask //遮罩的样式
  position: fixed
  top: 0
  left: 0
  width: 100%
  height: 100%
  z-index: 40
  backdrop-filter: blur(10px) //遮罩虚化
  opacity: 1
  background: rgba(7, 17, 27, 0.6)
  &.fade-enter-active, &.fade-leave-active //遮罩动画
    transition: all 0.5s
  &.fade-enter, &.fade-leave-active
    opacity: 0
    background: rgba(7, 17, 27, 0)
```
