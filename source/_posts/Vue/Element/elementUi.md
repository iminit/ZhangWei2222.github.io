---
title:  ElementUI
date: 2019-03-15 15:32:37
categories:
- Vue
tags:
- Element
comments: false
---


#### 表格组件怎么使用？它有哪些属性？ 

答：用一个<el-table>< /el-table>这个是最外面的上面，里面用<el-table-row></el-table-row>。

属性：
一、el-table组件的data是用来获取数据；
二、每一行el-table-row的数据通过prop属性。

#### 计数器组件怎么使用？
答：首先引入ui框架，然后放的vue里面写<el-input-num></el-input-num>

#### 表单组件怎么使用？它有哪些属性？
答：先写el-form组件，里面写el-form-item用来放输入框，例如用户名和密码那些。

属性：
一、el-from属性有ref、rules、绑定:model
二、el-form-item属性有prop用来定义一个该输入框对应的验证规则

#### 表单验证怎么实现？

答：
**表单验证 **
一、在一开始el-form中绑定:model="a"，遵守一个规则:rules="规则名b"，然后:ref跟之前的a一样值，要遵守规则的数据名。
二、在data中定义a，里面放表单的所有数据。
三、在data属性中定义rules规则，例如：必须填写就是required:true，最小最大就是min和max。
四、里面放el-form-item，上面定义prop取一个名字等于什么，rules中用这个prop里面定义的名字作为规则属性。
五、若要自定义规则，首先在rules的规则中定义validata属性，它的值就是自定义规则的名称，然后在data里写  var 自定义规则名称 = (rule,value,callback)=>  如果校验正确就返回callback()，校验错误就返回callback(ne Error('***有误！'))