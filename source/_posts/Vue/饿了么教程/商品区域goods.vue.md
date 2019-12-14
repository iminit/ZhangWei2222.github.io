---
title: 商品区域goods.vue
date: 2017-09-09 15:15:47
categories:
- Vue
tags:
- 饿了么教程
comments: false
---

2017-09-09
技术点:
- 定义了2个wrapper,分别是menu-wrapper和foods-wrapper,对应当前页面的架构,左右两边的区域
- v-if和v-show的选择使用
- v-for传递索引
- vue传递原生事件$event
- 使用stylus的mixin处理一些border和img的问题
- 建立menu区域和foods区域的Y坐标对应关系,实现滚动foods区域会显示相应的menu区域,点击某个menu区域就显示某个固定的foods区域
- flex布局的使用,实现foods区域的布局
- 技巧类:对一些需要js操作的class(但是又没有实际用途的)可以建立一个类似food-list-hook钩子类
- font-size为0的技术点,处理行内元素的间隙问题
- vue的$nextTick使用
- vue的$refs的使用
- vue的computed属性使用
- vue的class绑定使用
- 在一些地方里面,使用table的垂直居中会很简单实现垂直居中
- better-scroll的使用



html代码

```
<template>
  <div class="goods">
  //第一个区域,menu区域
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
      //v-for的使用,class绑定,传递原生事件
        <li v-for="(item,index) in goods" class="menu-item" :class="{'current':currentIndex === index}"
            @click="selectMenu(index,$event)">
          <span class="text border-1px">
          <!--v-show使用-->
            <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
          </span>
        </li>
      </ul>
    </div>
    //第二个区域 food区域
    //$refs的使用
    <div class="foods-wrapper" ref="foodsWrapper">
      <ul>
      //hook钩子类的使用(food-list-hook)
        <li v-for="item in goods" class="food-list food-list-hook">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="food in item.foods" class="food-item">
              <div class="icon">
                <img width="57" height="57" :src="food.icon">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="count">月售{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">¥{{food.price}}</span><span class="old"
                                                                v-show="food.oldPrice">¥{{food.price}}</span>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    //第三个区域,shopcart区域
    <shopcart ref="shopcart" :selectFoods="selectFoods" :deliveryPrice="seller.deliveryPrice"
              :minPrice="seller.minPrice"></shopcart>
    //第四个区域,隐藏的食物详情区域
    <food @add="addFood" :food="selectedFood" ref="food"></food>
  </div>
</template>
```

备注:

- v-for使用已经很常见了,不过这里需要了解,vue1和2有区别,现在是用vue2,所以index变量传递会变成现在这种模式(item,index) in goods
- vue传递原生事件使用$event
- :class=”{‘current’:currentIndex === index}”是vue的绑定class的使用方法,通过绑定一个class变量来直接操作,并且这里的逻辑会跟js代码里面对应
- 通过currentIndex和index做对比,来确认是否添加current类,他们之间的对比关系也就是menu区域和foods区域的显示区域的对比关系
- 通过添加current类来实现当前页面的区域的样式变化
- currentIndex是一个计算属性,可以随时变化并且直接反应到dom上(看js里面逻辑)
- v-show和v-if的区别官网已经说过
- v-if 是“真正的”条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。
- v-if 也是惰性的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。

一般来说， v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件不太可能改变，则使用 v-if 较好。

- $refs的使用是vue操作dom的一种方式:
    - ref 被用来给元素或子组件注册引用信息。引用信息将会注册在父组件的 $refs 对象上。
    - 如果在普通的 DOM 元素上使用，引用指向的就是 DOM 元素; 如果用在子组件上，引用就指向组件实例:
- hook钩子类的使用,需要结合js里面的语法来看,这个类只是用来操作,不会产生dom的渲染,方便js控制清晰.

js代码

```
<script>
  import BScroll from 'better-scroll'; //导入better-scroll
  import shopcart from '../../components/shopcart/shopcart';//导入shopcart购物车组件
  import cartcontrol from '../../components/cartcontrol/cartcontrol'; //导入购物组件
  import food from '../../components/food/food'; //导入食物详情组件

  const ERR_OK = 0; //常量,方便解耦

  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data(){
      return {
        goods: [],
        listHeight: [], //用来储存foods区域的各个区块的高度(clientHeight)
        scrollY: 0 //用来存储foods区域的滚动的Y坐标
        selectedFood: {} //用来存储当前已被选择的food数据,对象保存形式
      }
    },
    computed: {
      currentIndex(){ //计算到达哪个区域的区间的时候的对应的索引值
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i]; //当前menu子块的高度
          let height2 = this.listHeight[i + 1]; //下一个menu子块的高度
          //滚动到底部的时候,height2为undefined,需要考虑这种情况
          //需要确定是在两个menu子块的高度区间
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            return i; //返回这个menu子块的索引
          }
        }
        return 0;
      },
      selectFoods() { //自动将所有的goods.food添加一个count属性,方便做数量运算
        let foods = [];
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.count) {
              foods.push(food);
            }
          });
        });
        return foods;
      }
    },
    created(){
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];

      this.$http.get('/api/goods').then((response) => {
        response = response.body;
        if (response.errno === ERR_OK) {
          this.goods = response.data;
          this.$nextTick(() => { //使用$nextTick来等待异步完成之后更新dom
            this._initScroll(); //绑定滚动dom
            this._calculateHeight(); //计算foods区域的各个区域的高度
          });
        }
      });
    },
    methods: {
      selectFood(food, event){
            if (!event._constructed) {//忽略掉BScroll的事件
              return;
            }
            this.selectedFood = food; //写入当前选择的food
            this.$refs.food.show(); //显示当前选择的food的详情页
      },
      selectMenu(index, event){
        if (!event._constructed) { //忽略掉BScroll的事件
          return;
        }
        let foodsList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
        let el = foodsList[index];
        //类似jump to的功能,通过这个方法,跳转到指定的dom
        this.foodsScroll.scrollToElement(el, 300);
      },
      addFood(target) {
        this._drop(target);
      },
      _drop(target) {
        // 体验优化,异步执行下落动画
        this.$nextTick(() => {
          this.$refs.shopcart.drop(target); //调用shopcart的下落动画
        });
      },
      _initScroll(){
      //初始化scroll区域
        this.menuScroll = new BScroll(this.$refs.menuWrapper, {
          click: true //结合BScroll的接口使用,是否将click事件传递,默认被拦截了
        });
        this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
          probeType: 3 //结合BScroll的接口使用,3实时派发scroll事件
        });
        //结合BScroll的接口使用,监听scroll事件(实时派发的),并获取鼠标坐标
        this.foodsScroll.on('scroll', (pos) => { 
          this.scrollY = Math.abs(Math.round(pos.y));//滚动坐标会出现负的,并且是小数,所以需要处理一下
        })
      },
      //计算foods内部块的高度
      _calculateHeight(){
        let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook'); //获取每一个food的dom对象
        let height = 0;
        this.listHeight.push(height); //初始化第一个高度为0
        for (let i = 0; i < foodList.length; i++) {
          let item = foodList[i]; //每一个item都是刚才获取的food的每一个dom
          height += item.clientHeight; //主要是为了获取每一个foods内部块的高度
          this.listHeight.push(height);
        }
      }
    },
    components: {
      shopcart,
      cartcontrol,
      food
    }
  }
</script>
```

备注:

- 是通过selectFood方法写入到vue实例里面,然后传给子组件food
- <shopcart ref=”shopcart” :selectFoods=”selectFoods” 这里selectFoods被自动添加了count属性,是为了让购物车更加简单的计算已选择的food
- 这里最关键的是menu和food两个区域的对应处理:
    - 在vue实例生命周期的开始created分别加载_initScroll和_calculateHeight
    - 通过_calculateHeight计算foods内部每一个块的高度,组成一个数组listHeight
    - 在_initScroll里面,设置了bscroll插件的一个监听事件scroll,将food区域当前的滚动到的位置的y坐标设置到一个vue实例属性scrollY this.scrollY = Math.abs(Math.round(pos.y));
    - 通过计算属性currentIndex,获取到food滚动区域对应的menu区域的子块的索引,然后通过设置一个class来做样式切换变化:class="{'current':currentIndex === index},实现联动
    - 另外当点击menu 区域的时候,会触发selectMenu事件,也会根据点击到的menu子块的索引然后去触发food区域滚动到对应的高度区块区间this.foodsScroll.scrollToElement(el, 300);
    - 这样完成整个对应.
- 抛物线小球动画横跨多个vue组件(也可以说是横跨了多个DOM),整个动画会在关于购物车添加按钮的动画里面说
- better-scroll需要安装,npm安装,具体参看better-scroll官网
- 关于在selectMenu中点击,在pc界面会出现两次事件,在移动端就只出现一次事件的问题:
    - 原因:
    - bsScrooler会监听事件(例如touchmove,click之类),并且阻止默认事件(prevent stop),并且他只会监听移动端的,pc端的没有监听
    - 在pc页面上 bsScroller也派发了一次click事件,原生也派发了一次click事件
    -         //bsScroll的事件,有_constructed: true
    -         MouseEvent {isTrusted: false, _constructed: true, screenX: 0, screenY: 0, clientX: 0…}
    -         //pc的事件
    -         MouseEvent {isTrusted: true, screenX: -1867, screenY: 520, clientX: 53, clientY: 400…}    
- 解决:
    - 针对bsScroole的事件,有_constructed: true,所以做处理,return掉非bsScroll的事件

css代码

```
<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl"

  .goods
    display: flex
    position: absolute //根据设计图实现,
    top: 174px //去掉header和tab的位置
    bottom: 46px //预留购物车位置
    width: 100%
    overflow: hidden
    .menu-wrapper
      flex: 0 0 80px //flex布局预留空间
      width: 80px //兼容android浏览器,不加的话会没办法预留空间
      background: #f3f5f7
      .menu-item
        display: table
        height: 54px
        width: 56px
        padding: 0 12px
        line-height: 14px
        &.current
          position: relative
          z-index: 10
          margin-top: -1px
          background: #ffffff
          font-weight: 700
          .text
            border-none()
        .icon
          display: inline-block
          vertical-align: top
          width: 12px
          height: 12px
          margin-right: 2px
          background-size: 12px 12px
          background-repeat: no-repeat
          &.decrease
            bg-image('decrease_3')
          &.discount
            bg-image('discount_3')
          &.guarantee
            bg-image('guarantee_3')
          &.invoice
            bg-image('invoice_3')
          &.special
            bg-image('special_3')
        .text
          display: table-cell
          width: 56px
          vertical-align: middle //table默认支持垂直居中
          border-1px(rgba(7, 17, 27, 0.1))
          font-size: 12px
    .foods-wrapper
      flex: 1
      .title
        padding-left: 14px
        height: 26px
        line-height: 26px
        border-left: 2px solid #d9dde1
        font-size: 12px
        color: rgb(147, 153, 159)
        background: #f3f5f7
      .food-item
        display: flex
        margin: 18px
        padding-bottom: 18px
        border-1px: (rgba(7, 17, 27, 0.1))
        &:last-child
          border-none() 
          margin-bottom: 0
        .icon
          flex: 0 0 57px
          margin-right: 10px
        .content
          flex: 1 //flex布局等分剩下空间
          .name
            margin: 2px 0 8px 0
            height: 14px
            line-height: 14px
            font-size: 14px
            color: rgb(7, 17, 27)
          .desc, .extra
            line-height: 10px
            font-size: 10px
            color: rgb(147, 153, 159)
          .desc
            margin-bottom: 8px
            line-height: 12px
          .extra
            .count
              margin-right: 12px
          .price
            font-weight: 700
            line-height: 24px
            .now
              margin-right: 8px
              font-size: 14px
              color: rgb(240, 20, 20)
            .old
              text-decoration: line-through
              font-size: 10px
              color: rgb(240, 20, 20)
        .cartcontrol-wrapper
            position: absolute
            right: 0
            bottom: 12px   
</style>
```
