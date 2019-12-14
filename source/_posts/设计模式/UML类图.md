---
title: 设计模式-六大原则
date: 2019-12-13 14:10:47
categories:
- 设计模式
comments: false
---

## UML 类图

#### 1. 继承

用一条**带空心箭头的直线**表示

![img](https://raw.githubusercontent.com/xietao3/Study-Plan/master/DesignPatterns/src/%E7%BB%A7%E6%89%BF.png)

#### 2. 实现

用一条**带空心箭头的虚线**表示

![img](https://raw.githubusercontent.com/xietao3/Study-Plan/master/DesignPatterns//src/%E5%AE%9E%E7%8E%B0.png)

#### 3.  组合

表示**整体由部分构成**的语义，是一种强依赖的特殊聚合关系，**如果整体不存在了，则部分也不存在了**

![img](https://raw.githubusercontent.com/xietao3/Study-Plan/master/DesignPatterns//src/%E7%BB%84%E5%90%88.png)

#### 4. 聚合

表示**整体由部分构成**的语义，例如一个部门由多个员工组成。与组合关系不同的是，整体和部分**不是强依赖**的，即**整体不存在了，部分仍然存在**

![img](https://raw.githubusercontent.com/xietao3/Study-Plan/master/DesignPatterns/src/%E8%81%9A%E5%90%88.png)

#### 5. 关联

用**一条直线**表示，描述不同类的对象之间的结构关系，它是一种静态关系， 通常与运行状态无关，一般由常识等因素决定的。它一般用来定义**对象之间静态的、天然的结构**， 所以，关联关系是一种“强关联”的关系。

比如，乘车人和车票之间就是一种关联关系，学生和学校就是一种关联关系，关联关系默认不强调方向，表示对象间相互知道。

![img](https://raw.githubusercontent.com/xietao3/Study-Plan/master/DesignPatterns/src/%E5%85%B3%E8%81%94.png)

#### 6. 依赖

用**一套带箭头的虚线**表示，如A依赖于B，他描述一个对象在运行期间会用到另一个对象的关系。

与关联关系不同的是，它是一种临时性的关系，通常在运行期间产生，并且随着运行时的变化，依赖关系也可能发生变化。显然，依赖也有方向，双向依赖是一种非常糟糕的结构，我们总是应该保持单向依赖，杜绝双向依赖的产生。

![img](https://raw.githubusercontent.com/xietao3/Study-Plan/master/DesignPatterns/src/%E4%BE%9D%E8%B5%96.png)

