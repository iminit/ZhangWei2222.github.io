---
title: 评价列表ratings组件
date: 2017-09-09 15:15:47
categories:
- Vue
tags:
- 饿了么教程
comments: false
---

需要注意的是main.js里面需要取消vue-router的默认路由,src/main.js的router.replace(‘/goods’);需要去掉

原因是在添加了其他页面(原来路由只有一个页面goods,现在多了页面ratings)之后,在浏览器加载页面的时候:

1. 首先会先加载所有页面
1. 在各个页面都在加载的过程中,会跳到默认路由
1. 然后这些页面被中断了加载,导致了页面内部的一些靠dom渲染的代码无法执行,例如bs-scroll
1. 所以会出现一些报错,TypeError: Cannot read property ‘children’ of undefined
1. 所以要做默认路由的时候,直接用url做,不用vue-router自己去跳转,直接写第一个url的地址

### 主体框架

```
<div class="ratings" ref="ratings">
 <div class="overview">

 </div>
<!--里面是所有的内容-->
</div>
```

需要加一个ref,为了让bscroll可以初始化dom
### ratings内容
html代码

```
<div class="ratings-content">
      <div class="overview">
        <div class="overview-left">
          <h1 class="score">{{seller.score}}</h1>
          <div class="title">综合评分</div>
          <div class="rank">高于周边商家{{seller.rankRate}}%</div>
        </div>
        <div class="overview-right">
        <!--三个一样的模块,服务态度,商品评分,送达时间-->
          <div class="score-wrapper">
            <span class="title">服务态度</span>
            <!--引入了star组件-->
            <star :size="36" :score="seller.serviceScore"></star>
            <span class="score">{{seller.serviceScore}}</span>
          </div>
          <div class="score-wrapper">
            <span class="title">商品评分</span>
            <star :size="36" :score="seller.foodScore"></star>
            <span class="score">{{seller.foodScore}}</span>
          </div>
          <div class="delivery-wrapper">
            <span class="title">送达时间</span>
            <span class="delivery">{{seller.deliveryTime}}分钟</span>
          </div>
        </div>
</div>
```

- 三个一样的模块共用一套样式代码,分别代入不同的数据
- 这里的数据都是获取seller的数据,是父组件传入的props

js代码

```
import star from '../star/star';

export default {
  props: {
    seller: {
      type: Object
    }
  },
  components: {
    star
  }
};
css代码
.ratings
  position: absolute
  top: 174px //留空一部分,给header使用
  bottom: 0
  left: 0
  width: 100%
  overflow: hidden
  .overview
    display: flex
    padding: 18px 0
    .overview-left
      flex: 0 0 137px //flex布局,固定左边,右边自动适配
      padding: 6px 0
      width: 137px
      border-right: 1px solid rgba(7, 17, 27, 0.1)
      text-align: center
      @media only screen and (max-width: 320px) //适配i5屏幕
        flex: 0 0 120px //适配的flex数值可以从根据设计稿计算
        width: 120px
      .score
        margin-bottom: 6px
        line-height: 28px
        font-size: 24px
        color: rgb(255, 153, 0)
      .title
        margin-bottom: 8px
        line-height: 12px
        font-size: 12px
        color: rgb(7, 17, 27)
      .rank
        line-height: 10px
        font-size: 10px
        color: rgb(147, 153, 159)
    .overview-right
      flex: 1 //flex布局,固定左边,右边自动适配
      padding: 6px 0 6px 24px
      @media only screen and (max-width: 320px) //适配i5屏幕
        padding-left: 6px
      .score-wrapper //三个一样的模块的共用样式
        margin-bottom: 8px
        font-size: 0
        .title
          display: inline-block
          line-height: 18px //针对模块内部的元素的对齐行,所以需要写到内部元素里面去
          vertical-align: top 
          font-size: 12px
          color: rgb(7, 17, 27)
        .star
          display: inline-block
          margin: 0 12px
          vertical-align: top
        .score
          display: inline-block
          line-height: 18px
          vertical-align: top
          font-size: 12px
          color: rgb(255, 153, 0)
      .delivery-wrapper
        font-size: 0
        .title
          line-height: 18px
          font-size: 12px
          color: rgb(7, 17, 27)
        .delivery
          margin-left: 12px
          font-size: 12px
          color: rgb(147, 153, 159)
```

关于适配iphone5,因为设计稿是以iphone6为模板设计的,如果不适配一些小的屏幕的话,对于一些比较宽的div(例如overview-left,overview-right)就会出现换行,被挤压的显示清空,所以需要使用media query来做一些基本的适配,这里只是以iphone5为适配参考,具体做法就是针对不同的屏幕宽度做处理

### ratingselect组件
这个引入类似ratingselect组件

html代码

```
<!--引入split组件-->
<split></split>
<!-- 引入ratingselect组件 -->
      <ratingselect @select="selectRating" @toggle="toggleContent" :selectType="selectType" :onlyContent="onlyContent"
                    :ratings="ratings"></ratingselect>
```

### ratings列表
html代码

```
<div class="rating-wrapper">
        <ul>
        <!--使用needShow方法控制显示-->
          <li v-for="rating in ratings" v-show="needShow(rating.rateType, rating.text)" class="rating-item">
            <div class="avatar">
              <img width="28" height="28" :src="rating.avatar">
            </div>
            <div class="content">
              <h1 class="name">{{rating.username}}</h1>
              <div class="star-wrapper">
              <!--引入star组件-->
                <star :size="24" :score="rating.score"></star>
                <span class="delivery" v-show="rating.deliveryTime">{{rating.deliveryTime}}</span>
              </div>
              <p class="text">{{rating.text}}</p>
              <!--recommend的处理,尤其注意class跟布局的使用-->
              <div class="recommend" v-show="rating.recommend && rating.recommend.length">
                <span class="icon-thumb_up"></span>
                <span class="item" v-for="item in rating.recommend">{{item}}</span>
              </div>
              <!--过滤时间格式-->
              <div class="time">
                {{rating.rateTime | formatDate}}
              </div>
            </div>
          </li>
        </ul>
</div>
```

- 需要注意recommend的布局处理
- needShow和过滤时间类似food.vue组件里面使用九.商品详情页food.vue

js代码

```
import BScroll from 'better-scroll';
import { formatDate } from '../../common/js/date'; //相对路径导入
import star from '../star/star';

const ALL = 2; //设置常量,比较好的代码风格,代替直接写数字到代码里面去
const ERR_OK = 0;

export default {
  props: {
    seller: {
      type: Object
    }
  },
  data() {
    return {
      ratings: []
    };
  },
  created() { //初始化数据,从api那里获取
    this.$http.get('/api/ratings').then((response) => {
      response = response.body;
      if (response.errno === ERR_OK) {
        this.ratings = response.data;
        this.$nextTick(() => { //异步初始化滚动
          this.scroll = new BScroll(this.$refs.ratings, {
            click: true
          });
        });
      }
    });
  },
  methods: {
    needShow(type, text) { //控制显示是否有内容的rate
      if (this.onlyContent && !text) {
        return false;
      }
      if (this.selectType === ALL) {
        return true;
      } else {
        return type === this.selectType;
      }
    }
  },
  filters: { //过滤时间
    formatDate(time) {
      let date = new Date(time);
      return formatDate(date, 'yyyy-MM-dd hh:mm');
    }
  },
  components: {
    star
  }
};
```

css代码

```
@import "../../common/stylus/mixin.styl"

    .rating-wrapper
      padding: 0 18px
      .rating-item
        display: flex //使用flex布局
        padding: 18px 0
        border-1px(rgba(7, 17, 27, 0.1))
        .avatar
          flex: 0 0 28px //使用flex布局
          width: 28px
          margin-right: 12px
          img
            border-radius: 50%
        .content
          position: relative //重新定义相对布局的参考父div,被内部元素做绝对布局使用
          flex: 1 //使用flex布局
          .name
            margin-bottom: 4px
            line-height: 12px
            font-size: 10px
            color: rgb(7, 17, 27)
          .star-wrapper
            margin-bottom: 6px
            font-size: 0
            .star
              display: inline-block
              margin-right: 6px
              vertical-align: top
            .delivery
              display: inline-block
              vertical-align: top
              line-height: 12px
              font-size: 10px
              color: rgb(147, 153, 159)
          .text
            margin-bottom: 8px
            line-height: 18px
            color: rgb(7, 17, 27)
            font-size: 12px
          .recommend
            line-height: 16px
            font-size: 0
            .icon-thumb_up, .item //共有属性
              display: inline-block
              margin: 0 8px 4px 0 //分配右外边距和下外边距
              font-size: 9px
            .icon-thumb_up //个别属性,因为icon没有颜色,需要配置
              color: rgb(0, 160, 220)
            .item //个别属性
              padding: 0 6px //设置左右内边距,撑开布局,形成类似button的效果
              border: 1px solid rgba(7, 17, 27, 0.1)
              border-radius: 1px
              color: rgb(147, 153, 159)
              background: #fff
          .time
            position: absolute
            top: 0
            right: 0
            line-height: 12px
            font-size: 10px
            color: rgb(147, 153, 159)
```

这里的flex布局是左边固定28px,然后右边占满剩下的空间
- flex: 0 0 28px 是flex-grow为0(项目不放大比例),flex-shrink为0(项目不缩小比例),flex-basis为28px(固定占用28px)


- flex: 1是flex 1 1 auto的缩写,就是会放大项目比例,并且剩余的项目空间也占有
参考:Flex 布局教程：语法篇

针对recommend的布局:
- 每一个内容都是一个span,span是行内元素,可以并列一行


- 设置外边距是为了span之间能够形成独立的间隙


- 设置内边距是为了让span和文字形成button的效果