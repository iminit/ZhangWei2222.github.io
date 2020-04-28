---
title: MySQL-LEFT JOIN 多表联查
date: 2020-04-28 18:31:47
categories:
- MySQL
comments: false
---

> LEFT JOIN：返回包括左表中的所有的记录和右表连接字段相等的记录
>
> `select * from A left join B on A.id = B.id`
>
> 因为 LEFT JOIN 是以左表为主表，所以只要左表有数据，不管右表有没有数据。(如果右表没有数据则为 null ，查询结果都会存在）

<!-- more -->

### 二表联查

```mysql
SELECT a.name,b.value
FROM users AS a LEFT JOIN orders AS b ON a.id = b.userId
```



### 三表联查

```mysql
SELECT a.count,b.bname,c.age
from  users AS a LEFT JOIN orders AS b ON a.ruleid = b.id 
LEFT JOIN landlords as c ON a.topruleid = c.id
```

