---
title: Vue-watch
date: 2019-11-21 10:06:47
categories:
- Vue
comments: false
---

#### 立即执行

场景：比如搜索框，一般会在页面生成`created()`或`mounted()`生命周期调用接口方法获取全部数据，然后`watch`输入的`value`，再调用一次接口方法。代码如下

<!-- more -->

```js
export default {
	data(){
		return {
			sarchValue:""
		}
	},
	created():{
		this.getList();
	},
	methods:{
		getList(){
			server.getList({
				serachValue:this.searchValue
			})
		}
	},
	watch:{
		serachValue(){
			this.getList()
			//getList里面通过searchValue去搜索数据库
		}
	}
}

```

适用`immediate`:true，可以初始化时立即执行，也就是说`data`里面定义了`value`就立即执行`handle`，改后

```js
export default {
	data(){
		return {
			sarchValue:""
		}
	},
	created():{
	},
	methods:{
		getList(){
			server.getList({
				serachValue:this.searchValue
			})
		}
	},
	watch:{
		serachValue:{
			//使用watch值是对象的第三种情况
			handler:"getList"
			//getList里面通过searchValue去搜索数据库
			immediate:true
		}
	}
}
```

#### 深度监听

```js
watch:{
  inpValObj:{
    handler(newVal,oldVal){
      console.log(newVal, oldVal)    
    },
    deep:true
  }
}
```

深度监听可以监听对象的变化，但是无法监听具体对象的属性变化：如我们改变了`inpValObj`的属性，确实可以打印出`newVal, oldVal`，但是并不能看到具体属性的变化。

**方法：直接监听对象的属性**

```js
watch: {
  'obj.name'(curVal, oldVal) {
    // TODO
  }
}
```
