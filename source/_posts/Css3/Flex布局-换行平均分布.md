---
title: Flex布局-换行平均分布
date: 2020-02-22 11:30:37
categories:
- Css3
tags:
- Flex
comments: false
---

有一个需求：多个盒子需要换行布局，单纯使用 `flex` 的属性，会发现没有办法平均分布，中间会缺，记录一种解决思路

```css
// 以下写法，如果盒子数量不是列数的倍数，将会产生空白
display: flex;
display: -webkit-flex;
justify-content: space-between;
flex-direction: row;
flex-wrap: wrap;
```

<!-- more -->

解决办法：

```vue
<van-button
            v-for="item in toiletList"
            :key="item.id"
            type="default"
            size="small"
            @click="selectToilet(item.id)"
            :style="{'background':toiletChosen === item.id ? '#dcdee0':'#fff' }"
            >{{item.value}}</van-button>
<van-button
            type="default"
            size="small"
            v-for="(item, index) in 4"
            :key="index"
            style="visibility: hidden;height:0;border: 0;margin: 0;"
            >1</van-button>
```

简单地说，就是如果最后一行没满，v-for 循环补齐，并且把 visibility 设置成 hidden ，需要注意：一定要把作为补齐的盒子的高度设置为0 ，不然多出的会挤出来哦

