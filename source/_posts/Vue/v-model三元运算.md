---
title: v-model 三元运算
date: 2020-04-26 16:48:47
categories:
- Vue
comments: false
---

发现在 v-model 使用三元运算符绑定数据，不能够，比如 `v-model="name === '' ? text : name"`，如果 name 字段是空字符串时， 就让 input 绑定 text 字段，这样会报错。可以采取 `computed` 方式

```vue
<input type="text" v-model="xxx">

computed:{
    xxx(){
        if(this.name){
            return this.text;
        }else{
            return this.name;
        }
    }    
}
```

