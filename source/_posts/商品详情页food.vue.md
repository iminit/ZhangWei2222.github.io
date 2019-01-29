---
title: 商品详情页food.vue
date: 2017-09-09 15:15:47
categories:
- 前端技术篇
tags:
- VUE
- 饿了么
comments: false
---

### 主体结构
html代码

```
<template>
  <transition name="move">
  <!--要实现这个商品详情页的内容滚动,所以需要有一个显示标志和一个dom绑定-->
  <div v-show="showFlag" class="food" ref="food">
  </div>
  </transition>
</template>
```

- 用ref绑定food的DOM元素,为了被bscroll做滚动处理
- 用transition包裹了整个food,为了实现这个页面的进入和退出动画


js代码

```
import BScroll from 'better-scroll';
export default {
  props: {
    food: {
      type: Object
    }
  },
  data(){
    return {
      showFlag: false 
    }
  },
  methods: {
    show(){
      this.showFlag = true; 
      this.$nextTick(() => { //异步添加滚动绑定
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.food, {
            click: true
          });
        } else {
          this.scroll.refresh();
        }
      })
    },
    hide(){
      this.showFlag = false;
    }
}
```

- 通过跟hide方法切换showFlag的值来实现显示隐藏
- 因为整个页面是比较长的,需要做滚动,所以异步加载bscroll


css代码

```
.food
  position: fixed //霸占屏幕,全屏显示,所以用fixed布局
  left: 0
  top: 0
  bottom: 48px //保留底部购物车底部栏的位置
  z-index: 30 //z-index的数值是有考究的,要控制好各个页面的z-index纵深
  width: 100%
  background: #fff
  transform: translate3d(0, 0, 0)
  &.move-enter-active, &.move-leave-active //用vue的动画配置实现动画
    transition: all 0.2s linear
  &.move-enter, &.move-leave-active
    transform: translate3d(100%, 0, 0)
```

- 对于整个商品详情页的打开和关闭效果体验,需要对其做动画处理,transition
- 动画是3d变形,从右往左(改变的是x坐标),直线移动(linear)


### 商品封面
html代码

```
<img :src="food.image">
<!--有一个返回按钮,绑定一个hide方法-->
<div class="back" @click="hide">
<!--使用icon-->
  <i class="icon-arrow_lift"></i> 
</div>
```

js代码

```
export default {
  props: {
    food: { //用goods.vue组件传入的food数据
      type: Object
    }
  }
}
```

在goods.vue组件上传入的food数据

css代码

```
.image-header
position: relative
width: 100%
height: 0
padding-top: 100% //这是一个css-hack技巧
img
  position: absolute
  top: 0
  left: 0
  width: 100% //图片撑满整个img的div
  height: 100%
.back
  position: absolute
  top: 10px
  left: 0
  .icon-arrow_lift //返回按钮,使用的是icon的那个css
    display: block
    padding: 10px
    font-size: 20px
    color: #fff
```

在w3c规定里面,padding设置100%的时候,这个值的计算是相对于这个盒子模型的宽度计算的,然后现在是宽是100%,所以padding也是100%,然后使用的是padding-top,那么就是内上边距的高度就是宽度的值,所以这样就能自然撑开一个正方形,这样的目的在于页面打开的时候更自然,不会出现闪烁
### 商品标题
html代码



月售份
好评率%


￥￥








加入购物车


- 加入购物车按钮有一个动画fade,并且因为他的位置跟cartcontrol有重叠,避免穿透,所以直接.click.stop.prevent
- 加入购物车按钮的显示是通过判断food.count实现的,这个属性默认是没有的,当点击购物车按钮的时候强制生成Vue.set(this.food, ‘count’, 1);,这样做的目的是比较直观的增加一个属性去实现功能
- cartcontrol的使用跟一般组件使用差不多.

js代码

```
import BScroll from 'better-scroll';
import Vue from 'vue';
import cartcontrol from '../../components/cartcontrol/cartcontrol';

export default {
  props: {
    food: {
      type: Object
    }
  }
  methods: {
    addFirst(event){ //点击加入购物车按钮,传入事件
      if (!event._constructed) { //因为购物车按钮在bscroll里面,所以需要处理掉bscroll的事件类型
        return;
      }
      this.$emit('add', event.target); //触发当前实例food上的事件add(在goods组件上绑定在food组件的add方法)
      Vue.set(this.food, 'count', 1);
    },
    addFood(target) { //跟add关联的addFood方法
      this.$emit('add', target); //触发当前实例food上的事件add(在goods组件上绑定在food组件的add方法)
    }
  },
  components: {
    cartcontrol
  }
}
```

两次触发当前实例的事件add是因为两个操作都是同一个动作,这个动作是绑定在food组件上的add方法,而food组件会在goods组件中被导入,而在goods组件里面,addFood方法就会指向当前goods组件的方法_drop,继而使用shopcart的小球抛物线动画this.$refs.shopcart.drop(target);,这样就是实现了使用跨组件调用方法的效果.

css代码

```
.content
position: relative
padding: 18px
.title
  line-height: 14px
  margin-bottom: 8px
  font-size: 14px
  font-weight: 700
  color: rgb(7, 17, 27)
.detail
  margin-bottom: 18px
  line-height: 10px
  height: 10px
  font-size: 0 //注意行内元素的空格
  .sell-count, .rating
    font-size: 10px
    color: rgb(147, 153, 159)
  .sell-count
    margin-right: 12px
.price //这里的css样式就是在goods.vue出现过的样式,所以直接贴过来使用,也可以做成组件
  font-weight: 700
  line-height: 24px
  .now
    margin-right: 8px
    font-size: 14px
    color: rgb(240, 20, 20)
  .old
    text-decoration: line-through
    font-size: 10px
    color: rgb(147, 153, 159)
.cartcontrol-wrapper //固定cartcontrol组件的样式
  position: absolute
  right: 12px
  bottom: 12px
.buy //加入购物车按钮的样式
  position: absolute
  right: 18px
  bottom: 18px
  z-index: 10 //显示在cartcontrol的更外面
  height: 24px
  line-height: 24px
  padding: 0 12px
  box-sizing: border-box //不希望padding和border把整个div撑大
  border-radius: 12px
  font-size: 10px
  color: #fff
  background: rgb(0, 160, 220)
  opacity: 1 //加入动画,一个是为了体验,另外一个是为了延迟触发隐藏,避免小球抛物线动画出现问题
  &.fade-enter-active, &.fade-leave-active
    transition: all 0.2s
  &.fade-enter, &.fade-leave-active
    opacity: 0
    z-index: -1
```

- 这里的z-index是10,因为加入购物车按钮和cartcontrol的按钮重叠了,所以需要设置一个比较靠外的z坐标
- 这里的box-sizing可以直观的设置盒子模型的实际大小,而不用计算边框和内边距的影响,
- 这是box-sizing:border-size之后的图,注意这个auto,这是因为盒子的大小被固定了,边框和内边距也是固定的,然后内容会根据被固定的大小进行auto适配,这种从外到内的空间设置比较直观和方便
- 这里加入动画的原因是当点击加入购物车的按钮时候(addFirst),因为food.count被添加了数据Vue.set(this.food, ‘count’, 1);,所以加入购物车按钮会被隐藏(v-show的display:none),但同时会执行this.$emit(‘add’, event.target);,只不过这个是异步执行的,并且这个被异步执行的方法add是抛物线小球动画计算初始目标高度的地方,所以当传入的购物车按钮被设置为display:none的时候,动画的目标初始高度无法计算,就会去使用父层div的高度,从而影响了抛物线小球动画效果.
- - 设置一个动画延迟隐藏购物车按钮,从而给足够的时间vue将数据传递到异步执行的方法,不影响抛物线小球的动画初始目标计算
### 商品信息

```
<!--引入split组件负责隔离行-->
        <split v-show="food.info"></split>
        <div class="info" v-show="food.info">
          <h1 class="title">商品信息</h1>
          <p class="text">{{food.info}}</p>
        </div>
  import split from '../../components/split/split';

  export default {
    props: {
      food: {
        type: Object
      }
    },
    components: {
      split
    }
  }
    .info
      padding: 18px
      .title
        line-height: 14px
        margin-bottom: 6px
        font-size: 14px
        color: rgb(7, 17, 27)
      .text
        line-height: 24px
        padding: 0 8px
        font-size: 12px
        color: rgb(77, 85, 93)
```

### 商品评价
html代码

```
<div class="rating">
          <h1 class="title">商品评价</h1>
          <!--ratingselect组件-->
          <ratingselect @select="selectRating" @toggle="toggleContent" :selectType="selectType"
                        :onlyContent="onlyContent" :desc="desc"
                        :ratings="food.ratings"></ratingselect>
          <div class="rating-wrapper">
          <!--根据ratings长度显示ratings-->
            <ul v-show="food.ratings && food.ratings.length">
            <!--根据不同类型的rateType来切换不同类型的rate-->
              <li v-show="needShow(rating.rateType,rating.text)" v-for="rating in food.ratings" class="rating-item border-1px">
                <div class="user">
                  <span class="name">{{rating.username}}</span>
                  <img class="avatar" width="12" height="12" :src="rating.avatar">
                </div>
                <!--使用vue过滤器filter来处理时间-->
                <div class="time">{{rating.rateTime | formatDate}}</div>
                <p class="text">
                <!--根据不同类型的rateType来控制icon的显示-->
                  <span
                    :class="{'icon-thumb_up':rating.rateType===0,'icon-thumb_down':rating.rateType===1}"></span>
                    {{rating.text}}
                </p>
              </li>
            </ul>
            <!--没有rate的时候显示-->
            <div class="no-rating" v-show="!food.ratings || !food.ratings.length"></div>
          </div>
        </div>
```

- 绑定一个ratingselect组件,负责处理rate选择的
- 然后划分一个rating-wrapper区域,负责显示相关rate的信息
- 在子组件ratingselect选择的信息会影响到父组件的rating-wrapper区域内容的变化
    - needShow控制当前rate的内容显示,包括不同的type和是否排除没内容的rate
    - 绑定icon-thumb_up和icon-thumb_down的class来控制赞和批评的icon显示
    - 根据json数据里面的ratings数组来控制是否显示rate

js代码


```
const ALL = 2; //将控制type类转为常量控制

import Vue from 'vue';
import ratingselect from '../../components/ratingselect/ratingselect'; //引入ratingselect组件

export default {
  props: {
    food: {
      type: Object
    }
  },
  data(){
    return { //这些是传入ratingselect组件的数据,并且初始化值
      selectType: ALL, //默认所有rate
      onlyContent: true, //默认只显示有内容的rate
      desc: {
        all: '全部',
        positive: '推荐',
        negative: '吐槽'
      }
    }
  },
  methods: {
    needShow(type, text){ 
      if (this.onlyContent && !text) { //只显示有内容的 并且 没有内容就返回false
        return false;
      }

      if (this.selectType === ALL) { //显示全部类型的rate
        return true;
      } else { //只显示对应的类型的rate
        return type === this.selectType;
      }
    },
    selectRating(type) { //设置rate的类型
      this.selectType = type;
      this.$nextTick(() => {//切换的时候需要重新刷新bscroll
        this.scroll.refresh();
      });
    },
    toggleContent() { //切换显示是否有内容的rate
      this.onlyContent = !this.onlyContent;
      this.$nextTick(() => { //切换的时候需要重新刷新bscroll
        this.scroll.refresh();
      });
    }
  },
  components: {
    ratingselect
  }
}
```

- selectRating和toggleContent使用异步$nextTick是因为vue是异步更新dom的,当改变了vue属性时候,当前的dom不是立即更新的(会导致页面的高度变化了,但是bscroll来不及更新,影响滚动体验),而是会放进去异步更新队列里面等候更新,即使这个队列的等待时间不长,但是也来不及马上更新dom,所以使用$nextTick强制刷新这个队列
- 在food.vue组件使用selectRating和toggleContent来更新food.vue组件的属性,而不能在子组件ratingselect里面更新,因为vue限制了子组件不能更改父组件的属性,所以通过使用类似this.$emit(‘select’, type);来调用父组件的方法来更改

css代码

```
.rating
  padding-top: 18px
  .title
    line-height: 14px
    margin-bottom: 6px
    font-size: 14px
    color: rgb(7, 17, 27)
  .rating-wrapper
    padding: 0 18px
    .rating-item
      position: relative;
      padding: 16px 0;
      border-1px(rgba(7, 17, 27, 0.1))
      .user
        position: absolute
        right: 0
        top: 16px
        line-height: 12px
        font-size: 0 //注意行内元素的空隙
        .name
          display: inline-block
          margin-right: 6px
          vertical-align: top
          font-size: 10px
          color: rgb(147, 153, 159)
        .avatar
          border-radius: 50%
      .time
        margin-bottom: 6px
        line-height: 12px
        font-size: 10px
        color: rgb(147, 153, 159)
      .text
        line-height: 16px
        font-size: 12px
        color: rgb(7, 17, 27)
        .icon-thumb_up, .icon-thumb_down 
          margin-right: 4px
          line-height: 16px
          font-size: 12px
        .icon-thumb_up
          color: rgb(0, 160, 220)
        .icon-thumb_down
          color: rgb(147, 153, 159)
    .no-rating
      padding: 16px 0
      font-size: 12px
      color: rgb(147, 153, 159)
```

### 商品评价里的时间处理
split组件和formatDate组件

