---
title: SQL-BY与HAVING
date: 2019-08-24 10:14:37
categories:
- 数据库
tags:
- SQL
comments: false
---

## 前言
虽然都是BY，都是表条件（HAVING WHERE），它们有什么不同呢？？

## ORDER BY / GROUP BY 区别
### ORDER BY
- 行的排序（ASC正序，DESC倒序）

非常通俗易懂，ORDER BY 列名，就会返回列的顺/倒序排列

### GROUP BY
- 分组

不是行的排序！不是行的排序！不是行的排序！

必须与聚合函数一起用（sum()、count()、avg()等）

### 探究

#### 为啥必须与聚合函数一起用？

```sql
SELECT customer 
FROM Orders 
ORDER BY customer
返回customer列，不包括重复行

SELECT customer，price
FROM Orders 
ORDER BY customer
报错：把customer相同的元素聚合在一起，那么price就没有意义了

SELECT customer，SUM(price)
FROM Orders 
ORDER BY customer
此时把customer相同的元素聚合在一起，SUM(price)为聚合元素的总数
```

#### 聚合函数必须搭配GROUP BY吗？
```sql
SELECT Customer,SUM(OrderPrice) FROM Orders
```

### 注意
ORDER BY 要写在 GROUP BY 后面，对分组后的数据进行排序

Customer | SUM(OrderPrice)
---|---
Bush | 5700
Carter | 5700
Bush | 5700
Bush | 5700
Adams | 5700
Carter | 5700

因为指定了两列，分别返回了结果，Customer返回表中的所有行，SUM(OrderPrice)返回一个值，不能得到正确的答案。所以咱们必须后面加个GROUP BY，分组计算。

## HAVING
WHERE后面不能接聚合函数，以下写法会报错：

```sql
SELECT product,sum(price) 
FROM orders 
WHERE sum(price)>100
GROUP BY product
```

原因：聚合函数是针对结果集进行处理，而WHERE子句执行时结果集还没出来

解决： 对聚合函数限制条件时，需要写在 HAVING 后面；且 HAVING 只能在 GROUP BY 后面，对分组后的结果进行筛选

## 综合使用
如果一个查询语句同时出现 WHERE、GROUP BY、HAVING、ORDER BY时，编写和执行顺序：

- 用 WHERE 进行初步筛选，返回第 1 个结果集
- 第 1 个结果集使用 GROUP BY 分组，返回第 2 个结果集
- 第 2 个结果集使用 HAVING 进行筛选，返回第 3 个结果集
- 第 3 个结果集使用 ORDER BY 排序

```sql
SELECT product,sum(price) 
FORM orders 
WHERE product='Bush'
GROUP BY product 
HAVING sum(price)>100 
ORDER BY sum(price)
```