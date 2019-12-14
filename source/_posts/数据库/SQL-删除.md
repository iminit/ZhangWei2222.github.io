---
title: SQL-删除
date: 2019-09-04 15:49:37
categories:
- 数据库
tags:
- SQL
comments: false
---

## 前言

SQL语法中，DROP、DELETE、TRUNCATE都表示删除，下面对其进行区别

<!-- more -->

## 语法

```sql
DROP TABLE 表名
DELETE TABLE 表名 WHERE 列名=值
TRUNCATE TABLE 表名
```



## 区别

| DROP（删除表）                                               | DELETE（删除表中的数据）                                     | TRUNCATE（删除表中的数据）                                   |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **把整个表删除**：包括被依赖的约束、触发器、索引；依赖于该表的存储过程、函数会保留，但状态变为 invalid | **删除表中的行**，会把删除操作作为事务记录在日志中保存，以便进行**回滚操作** | **删除内容**，保留表的数据结构                               |
|                                                              |                                                              | 作为外键的表，不能TRUNCATE，而应该使用不带 WHERE 的DELETE    |
| 数据库定义语言（ddl）<br />操作立即生效<br />原数据不放到 rollback segment 中，不能回滚<br />操作不触发 trigger | 数据库操作语言（dml）<br />事务提交之后才生效<br />会放到 rollback segment 中，能回滚<br />操作执行时将触发 trigger | 数据库定义语言（ddl）<br />操作立即生效<br />原数据不放到 rollback segment 中，不能回滚<br />操作不触发 trigger |

**执行速度**：DROP > TRUNCATE > DELETE