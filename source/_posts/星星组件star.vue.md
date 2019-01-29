---
title: 星星组件star.vue
date: 2017-09-09 15:15:47
categories:
- 前端技术篇
tags:
- VUE
- 饿了么
comments: false
---


整个流程是:

1. 绑定星星类型的class(48,36,24尺寸),使用starType
1. 使用class来显示星星,有3种类型,全星,半星,无星,使用star-item代表星星本身,然后分别使用on,off,half代表三种不同类型的星星
1. 一个span代表一个星星项目,并且使用v-for循环将星星项目输出

组合出来的星星html就类似这样


```
<div class="star star-48">
<span class="star-item on"></span>
<span class="star-item on"></span>
<span class="star-item on"></span>
<span class="star-item on"></span>
<span class="star-item half"></span>
</div>
```

html部分

```
<template>
  <div class="star" :class="starType">
    <span v-for="itemClass in itemClasses" :class="itemClass" class="star-item"></span>
  </div>
</template>
```

js部分
- 设置常量是为了方便解耦
- 星星计算比较巧妙(根据分数转换为星星数)
    - 对于分数score进行2然后向下取整,然后再除以2,是为了获取所有星星的数量,并且这个数量是0.5倍数的,例如4.6 2就是9.2,然后向下取整是9,然后再除以2就是4.5,那么就可以得到一个0.5倍数的星星数,可以转换为4个全星+一个半星
    - 对于非整数的星星算作是半个星星,需要知道是否有存在这种情况,所以分数score%1 ,例如8 % 1是0, 8.5 % 1就不是0,并且这个半星只会出现一次,因为半星状态就只要一个
    - 没有星星的部分是要补全的,这里使用while循环来处理这种情况


```
<script>
  //设置常量
  const LENGTH = 5;
  const CLS_ON = 'on';
  const CLS_HALF = 'half';
  const CLS_OFF = 'off';

  export default{
    props: {
      size: { //传入的size变量
        type: Number //设置变量类型
      },
      score: { //传入的score变量
        type: Number
      }
    },
    computed: {
      starType(){ //通过计算属性,返回组装过的类型,用来对应class类型
        return 'star-' + this.size;
      },
      itemClasses(){
        let result = []; //返回的是一个数组,用来遍历输出星星
        let score = Math.floor(this.score * 2) / 2; //计算所有星星的数量
        let hasDecimal = score % 1 !== 0; //非整数星星判断
        let integer = Math.floor(score); //整数星星判断
        for (let i = 0; i < integer; i++) { //整数星星使用on
          result.push(CLS_ON);//一个整数星星就push一个CLS_ON到数组
        }
        if (hasDecimal) { //非整数星星使用half
          result.push(CLS_HALF);//类似
        }
        while (result.length < LENGTH) { //余下的用无星星补全,使用off
          result.push(CLS_OFF);//类似
        }
        return result;
      }
    }
  }
</script>
```

css部分
- 引入mixin.styl是为了使用bg-image的mixin,因为之前做了一个mixin是专门处理2x和3x图片的转换
- 因为这里有3种类型的星星图片,分别是48尺寸,36尺寸,24尺寸,所以对于每一个类别的图片分别使用一种class做对应
- 每一种星星的尺寸都是有一种相对应的图片的,例如48尺寸的星星就会有,并且图片放在相对应的vue文件目录下


```
star48_half@2x.png
star48_half@3x.png
star48_off@2x.png
star48_off@3x.png
star48_on@2x.png
star48_on@3x.png
<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl" //引入mixin文件

  .star
    .star-item
      display: inline-block
      background-repeat: no-repeat
    &.star-48 //48尺寸的星星
      .star-item //每一个星星的基本css信息
        width: 20px
        height: 20px
        margin-right: 22px //每一个星星dom都有外边距
        background-size: 20px 20px
        &:last-child //最后一个的外边距就是0
          margin-right: 0
        &.on //全星状态的class
          bg-image('star48_on')
        &.half //半星状态的class
          bg-image('star48_half')
        &.off //无星状态的class
          bg-image('star48_off')
    &.star-36
      .star-item
        width: 15px
        height: 15px
        margin-right: 6px
        background-size: 15px 15px
      &:last-child
        margin-right: 0
      &.on
        bg-image('star36_on')
      &.half
        bg-image('star36_half')
      &.off
        bg-image('star36_off')
    &.star-24
      .star-item
        width: 10px
        height: 10px
        margin-right: 3px
        background-size: 10px 10px
      &:last-child
        margin-right: 0
      &.on
        bg-image('star24_on')
      &.half
        bg-image('star24_half')
      &.off
        bg-image('star24_off')
</style>
```
