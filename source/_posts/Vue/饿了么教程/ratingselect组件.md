---
title: ratingselect组件
date: 2017-09-09 15:15:47
categories:
- Vue
tags:
- 饿了么教程
comments: false
---

html代码

> 备注:父组件food.vue传入的数据


```
<ratingselect @select="selectRating" @toggle="toggleContent" :selectType="selectType"
:onlyContent="onlyContent" :desc="desc"
:ratings="food.ratings"></ratingselect>
```

- 方法有:@select=”selectRating” @toggle=”toggleContent”,通过将子组件的方法和父组件的方法进行关联,这样就能够实现跨组件通讯和操作
- 属性有::selectType=”selectType”:onlyContent=”onlyContent” :desc=”desc”:ratings=”food.ratings”,这是通过pros传入到子组件的属性,将父组件的数据传到子组件里面,也带有一种通过父组件来初始化子组件属性的意思.

### ratingselect的html代码

```
<div class="ratingselect">
<!--有使用一个border-1px的mixin-->
  <div class="rating-type border-1px">
  <!--绑定一个select方法控制切换,绑定class控制切换之后的按钮样式显示-->
    <span @click="select(2,$event)" class="block positive" :class="{'active':selectType ===2}">{{desc.all}}<span
      class="count">{{ratings.length}}</span></span>
    <span @click="select(0,$event)" class="block positive" :class="{'active':selectType ===0}">{{desc.positive}}<span
      class="count">{{positives.length}}</span></span>
    <span @click="select(1,$event)" class="block negative" :class="{'active':selectType ===1}">{{desc.negative}}<span
      class="count">{{negatives.length}}</span></span>
  </div>
  <!--绑定一个toggleContent方法来控制有内容和无内容的显示-->
  <div @click="toggleContent" class="switch" :class="{'on':onlyContent}">
    <span class="icon-check_circle"></span>
    <span class="text">只看有内容的评价</span>
  </div>
</div>
```

- @click=”select(2,$event)” select方法传入类型和事件,然后在methods里面调用父组件的方法,实现子组件控制父组件的目的
- :class=”{‘active’:selectType ===2}” 根据类型来确定显示的class,实现不同类型显示不同样式的目的
- positives.length 使用计算属性自动计算类型数组的长度,用来显示不同类型的数量
- @click=”toggleContent” :class=”{‘on’:onlyContent}”
    - toggleContent控制是否展示有内容的rate,也是在methods里面调用父组件的方法,实现子组件控制父组件的目的
    - 绑定on这个class来控制该按钮的样式

js代码

```
const POSITIVE = 0; //设置显示常量
const NEGATIVE = 1;
const ALL = 2;

export default{
  props: {
    ratings: { //传入ratings数组,跟food.ratings关联
      type: Array,
      default(){
        return [];
      }
    },
    selectType: { //跟selectType关联,通过在父组件里面设置这3个值来实现控制子组件的操作
      type: Number,
      default: ALL
    },
    onlyContent: { //跟onlyContent关联
      type: Boolean,
      default: true
    },
    desc: { //跟desc关联
      type: Object,
      default(){
        return {
          all: '全部',
          positive: '满意',
          negative: '不满意'
        }
      }
    }
  },
  computed: {
    positives(){ //自动过滤rateType(正向的rate)
      return this.ratings.filter((rating) => { //js的filter函数会返回一个处理后的(为true)结果的结果数组
        return rating.rateType === POSITIVE;
      })
    },
    negatives(){ //自动过滤rateType(反向的rate)
      return this.ratings.filter((rating) => {
        return rating.rateType === NEGATIVE;
      })
    }
  },
  methods: {
    select(type, event) { //选择rateType并且通知父组件
      if (!event._constructed) {
        return;
      }
      this.$emit('select', type); //调用父组件的方法
    },
    toggleContent(event) { //选择是否显示有内容的rate,并且通知父组件
      if (!event._constructed) {
        return;
      }
      this.$emit('toggle');
    }
  }
}
```

css代码

```
@import "../../common/stylus/mixin.styl"

.ratingselect
  .rating-type
    padding: 18px 0
    margin: 0 18px
    border-1px(rgba(7, 17, 27, 0.1)) //1像素边框的mixin
    font-size: 0 //去除行内元素间隙
    .block
      display: inline-block
      padding 8px 12px
      margin-right: 8px
      border-radius: 1px
      line-height: 16px
      font-size: 12px
      color: rgb(77, 85, 93)
      &.active //被选中的按钮的class(由:class控制)
        color: #ffffff
      .count
        margin-left: 2px
        font-size: 8px
      &.positive //设置支持的按钮的样式
        background: rgba(0, 160, 220, 0.2)
        &.active
          background: rgb(0, 160, 220)
      &.negative
        background: rgba(77, 85, 93, 0.2)
        &.active
          background: rgb(77, 85, 93)
  .switch
    padding: 12px 18px
    line-height: 24px
    border-bottom: 1px solid rgba(7, 17, 27, 0.1)
    color: rgb(147, 153, 159)
    font-size: 0
    &.on //on的class样式
      .icon-check_circle
        color: #00c850
    .icon-check_circle
      display: inline-block
      vertical-align: top
      margin-right: 4px
      font-size: 24px
    .text
      vertical-align: top
      font-size: 12px
```
