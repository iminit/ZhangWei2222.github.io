---
title: 商家页面seller.vue
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
<div class="seller" ref="seller">
    <div class="seller-content">
      <!--overview部分-->
      <div class="overview">
    
      </div>
      <split></split>
      <!--bulltin部分-->
      <div class="bulletin">
    
      </div>
      <split></split>
      <!--pics部分-->
      <div class="pics">
    
      </div>
      <split></split>
      <!--info部分-->
      <div class="info">
    
      </div>
    </div>
</div>
```

seller-contentdiv是能够扩大高度的,能够被内容增加而撑高,而seller div是固定的高度的,从而形成滚动

js代码

```
import BScroll from 'better-scroll';
import split from '../split/split';

export default {
  props: {
    seller: { //app.vue里面router-view会传入一个seller
      type: Object
    }
  },
  watch: { //当seller变动的时候会触发
    'seller'() {
      this.$nextTick(() => {
        this._initScroll();
        this._initPics();
      });
    }
  },
  mounted() { //在dom被渲染之后触发
    this.$nextTick(() => {
      this._initScroll();
      this._initPics();
    });
  },
  methods: {
    _initScroll() {
      if (!this.scroll) {
        this.scroll = new BScroll(this.$refs.seller, {
          click: true
        });
      } else {
        this.scroll.refresh();
      }
    },
    _initPics() {
      //详细代码在下面,这里暂时先忽略
    }
  },
  components: {
    split
  }
};
```

- 使用watch方法是为了监听数据seller的变化,因为当打开页面的时候,seller是异步获取的,并不一定是能够马上获取,没有seller的数据,相关dom就无法被渲染,并且bscroll是基于dom执行的,所以需要监听seller的变化然后来重新执行相关的初始化函数
- 使用mounted方法是为了保证相关dom渲染完成,因为bscroll是基于dom执行的,但是当切换页面的时候,dom会重新渲染,但未必能够马上完成,所以需要在mounted方法里面重新执行相关的初始化函数
- 关于watch和mounted的使用要基于vue的生命周期来理解vue生命周期

css代码

```
.seller
  position: absolute
  top: 174px //给header留空间
  bottom: 0
  left: 0
  width: 100%
  overflow: hidden //隐藏多余部分,因为需要滚动
```

### overview部分
html部分

```
<div class="overview">
  <h1 class="title">{{seller.name}}</h1>
  <div class="desc border-1px">
    <star :size="36" :score="seller.score"></star>
    <span class="text">({{seller.ratingCount}})</span>
    <span class="text">月售{{seller.sellCount}}单</span>
  </div>
  <!--列表处理remark内容-->
  <ul class="remark">
  <!--使用一个block块包裹内容-->
    <li class="block">
    <!--标题用h2标签-->
      <h2>起送价</h2>
      <!--用content块包裹内容-->
      <div class="content">
      <!--用span代表特殊字体-->
        <span class="stress">{{seller.minPrice}}</span>元
      </div>
    </li>
    <li class="block">
      <h2>商家配送</h2>
      <div class="content">
        <span class="stress">{{seller.deliveryPrice}}</span>元
      </div>
    </li>
    <li class="block">
      <h2>平均配送时间</h2>
      <div class="content">
        <span class="stress">{{seller.deliveryTime}}</span>分钟
      </div>
    </li>
  </ul>
  <!--收藏按钮-->
  <div class="favorite" @click="toggleFavorite">
    <span class="icon-favorite" :class="{'active':favorite}"></span>
    <span class="text">{{favoriteText}}</span>
  </div>
</div>
```

js部分

```
import BScroll from 'better-scroll';
//引入一个利用html5的localstorage的存储模块
import { saveToLocal, loadFromLocal } from 'common/js/store';

export default {
  props: {
    seller: {
      type: Object
    }
  },
  data() {
    return {
      favorite: (() => { 
      //利用localstorage读取这个属性
        return loadFromLocal(this.seller.id, 'favorite', false);
      })()//vue需要返回的data必须是函数,所以加上()
    };
  },
  computed: {
    favoriteText() { //通过favorite的值来计算favoriteText的值
      return this.favorite ? '已收藏' : '收藏';
    }
  },
  methods: {
    toggleFavorite(event) {
      if (!event._constructed) {
        return;
      }
      //通过取反来设置切换
      this.favorite = !this.favorite;
      //利用localstorage存储这个属性
      saveToLocal(this.seller.id, 'favorite', this.favorite);
    }
  }
};
```

store.js参考:split,formatDate,store,util组件

css部分

```
.overview
position: relative //设置相对布局参考位置
padding: 18px
.title
  margin-bottom: 8px
  line-height: 14px
  color: rgb(7, 17, 27)
  font-size: 14px
.desc
  padding-bottom: 18px
  border-1px(rgba(7, 17, 27, 0.1))
  font-size: 0
  .star
    display: inline-block
    margin-right: 8px
    vertical-align: top //行内对齐
  .text
    display: inline-block
    margin-right: 12px
    line-height: 18px
    vertical-align: top //行内对齐
    font-size: 10px
    color: rgb(77, 85, 93)
.remark
  display: flex //flex布局
  padding-top: 18px
  .block
    flex: 1 //flex布局等分区域
    text-align: center
    border-right: 1px solid rgba(7, 17, 27, 0.1)
    &:last-child
      border: none
    h2
      margin-bottom: 4px
      line-height: 10px
      font-size: 10px
      color: rgb(147, 153, 159)
    .content //设置该块的公共属性
      line-height: 24px
      font-size: 10px
      color: rgb(7, 17, 27)
      .stress //特殊大小再插入class覆盖
        font-size: 24px
.favorite
  position: absolute
  width: 50px //设置一个固定宽度,被text-align使用
  right: 11px
  top: 18px
  text-align: center
  .icon-favorite
    display: block
    margin-bottom: 4px
    line-height: 24px
    font-size: 24px
    color: #d4d6d9
    &.active
      color: rgb(240, 20, 20)
  .text
    line-height: 10px
    font-size: 10px
    color: rgb(77, 85, 93)
```

### bulletin部分
html代码

公告与活动











> classMap的原理(classMap使用在之前的header组件使用过Header.vue)

- seller.supports的结构是这样的

```
“supports”: [
  {
    "type": 0,
    "description": "在线支付满28减5"
  },
  {
    "type": 1,
    "description": "VC无限橙果汁全场8折"
  },
  {
    "type": 2,
    "description": "单人精彩套餐"
  },
  {
    "type": 3,
    "description": "该商家支持发票,请下单写好发票抬头"
  },
  {
    "type": 4,
    "description": "已加入“外卖保”计划,食品安全保障"
  }
],
```

- classMap是[‘decrease’, ‘discount’, ‘special’, ‘invoice’, ‘guarantee’]这样的,这个顺序是按照设计需要和数据结构排的,例如type为0对应在线支付满28减5
- 如果要获取对应的class,就可以classMap[seller.supports[index].type],例如seller.supports[0].type就是0,那么就是classMap[0]就是decrease这个class
- 如果要获取对应的description就可以seller.supports[index].description,例如seller.supports[0].description就直接知道了supports数组的第一个元素的description属性的值了
- 
js代码

```
import split from '../split/split';

export default {
  props: {
    seller: {
      type: Object
    }
  },
  created() { //使用自建的一个classMap数组来对应不同的类别
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
  },
  components: {
    split
  }
};
```

使用created方法是为了在加载的时候第一时间完成,因为vue的渲染需要this.classMap属性(created是vue的生命周期的第一个)

css代码


```
@import “../../common/stylus/mixin.styl”
.bulletin
  padding: 18px 18px 0 18px
  .title
    margin-bottom: 8px
    line-height: 14px
    color: rgb(7, 17, 27)
    font-size: 14px
  .content-wrapper
    padding: 0 12px 16px 12px
    border-1px(rgba(7, 17, 27, 0.1))
    .content
      line-height: 24px
      font-size: 12px
      color: rgb(240, 20, 20)
  .supports
    .support-item
      padding: 16px 12px
      border-1px(rgba(7, 17, 27, 0.1))
      font-size: 0
      &:last-child
        border-none() //需要引入一个mixin,处理没边框的问题
    .icon
      display: inline-block
      width: 16px
      height: 16px
      vertical-align: top
      margin-right: 6px
      background-size: 16px 16px
      background-repeat: no-repeat
      &.decrease //使用设计要求的大小的图标
        bg-image('decrease_4') //使用mixin处理这些图标
      &.discount
        bg-image('discount_4')
      &.guarantee
        bg-image('guarantee_4')
      &.invoice
        bg-image('invoice_4')
      &.special
        bg-image('special_4')
    .text
      line-height: 16px
      font-size: 12px
      color: rgb(7, 17, 27)
```

### pics部分
html代码

商家实景











- ref获取picWrapper是为了滚动图片需要,父DOM是picWrapper,子DOM是picList
- ref获取picList是为了计算图片列表的总长度,用来设置图片列表的横向滚动,因为滚动的触发条件是子DOM比父DOM要”大”的时候才会触发,由于图片的数量是未知的,所以需要去计算,然后写入到picList

js代码

_initPics() { //编程习惯的前置下划线,代表内部使用的方法


```
if (this.seller.pics) {
  let picWidth = 120; //
  let margin = 6;
  //这是图片列表的总长度
  let width = (picWidth + margin) * this.seller.pics.length - margin;
  //设置图片列表的宽度等于列表的总长度
  this.$refs.picList.style.width = width + 'px';
  //异步绑定滚动
  this.$nextTick(() => {
    if (!this.picScroll) {
      this.picScroll = new BScroll(this.$refs.picWrapper, {
        scrollX: true, //bscroll的横向滚动属性
        eventPassthrough: 'vertical' //bscroll的过滤垂直滚动
      });
    } else {
      this.picScroll.refresh();
    }
  });
}
```

- 一般来说,当某页面本身是垂直滚动的时候,中间的某个DOM需要做横向滚动的话,需要处理滚动坐标的变化,例如手势滚动的时候有x,y坐标,是向上,下,左还是右会区分
- 图片列表的总长度是单个图片的宽度+单个图片的外边距的和乘以图片的数量,再减去最后一个图片的外边距
- BScroll插件需要增加一个属性scrollX: true来传入横向的滚动坐标,也需要增加一个属性eventPassthrough: ‘vertical’过滤垂直的滚动,前者已经解释过,后者是因为需要横向滚动的页面是内嵌在垂直滚动的页面的里面的,所以会造成外面的垂直滚动事件也会传入到横向滚动的页面,为了避免影响横向滚动,所以需要屏蔽垂直滚动的事件
- 我们平时在做这种滚动内嵌滚动的时候也可以参考这个处理方法.
- 另外异步绑定滚动的常规做法,先判断是否已经有BScroll实例了,然后再确定是否需要刷新

css代码

```
.pics
padding: 18px
.title
  margin-bottom: 12px
  line-height: 14px
  color: rgb(7, 17, 27)
  font-size: 14px
.pic-wrapper
  width: 100% //图片列表区域是屏幕宽度
  overflow: hidden //隐藏多余部分
  white-space: nowrap  //连续的空白符会被合并
  .pic-list
    font-size: 0 //行内元素取消间隙
    .pic-item
      display: inline-block
      margin-right: 6px
      width: 120px
      height: 90px
      &:last-child //最后一个图片没有外边距
        margin: 0
```

### info部分
html代码

```
<div class="info">
  <h1 class="title border-1px">商家信息</h1>
  <ul>
    <li class="info-item" v-for="info in seller.infos">{{info}}</li>
  </ul>
</div>
```

css代码

```
.info
padding: 18px 18px 0 18px
color: rgb(7, 17, 27)
.title
  padding-bottom: 12px
  line-height: 14px
  border-1px(rgba(7, 17, 27, 0.1))
  font-size: 14px
.info-item
  padding: 16px 12px
  line-height: 16px
  border-1px(rgba(7, 17, 27, 0.1)) //1像素的border 的mixin
  font-size: 12px
  &:last-child
    border-none() //没有border的mixin
```

