---
title: SQL-简单操作语法
date: 2019-08-20 19:38:37
categories:
- 数据库
tags:
- SQL
comments: false
---

## 前言

希望有一个简单明了，忘记即查即记起的sql笔记，因为sql基础知识其实挺细挺多的，为了不混淆，分3部分进行梳理。

又引申了一个思考：虽然很多知识网上确实总结了许多，还很不一样，但是要变成自己的知识，当然不能简单的复制粘贴，而且也不一定某个人的总结就是对的，需要多次查阅，进行比较。用自己的语言进行复述，梳理，往后直接查阅笔记就省了很多猜疑与困惑的点。

<!-- more -->

## 介绍

- SQL —— Structured Query Language（结构化查询语句）
- 数据库上执行的大部分工作都由SQL语句完成
- SQL对大小写不敏感
- SQL由DDL( 数据定义语言 )与DML( 数据操作语言 )组成

## DDL —— 数据定义语言

-  创建数据库 / 表 / 索引（搜索键）/ 视图
	- CREATE DATABASE / TABLE  /  INDEX  / VIEW
- 修改数据库 / 表
	- ALTER DATABASE / TABLE 
- 删除数据库 / 表 / 索引（搜索键）/ 视图
	- DROP DATABASE / TABLE /  INDEX / VIEW 

### CREATE TABLE  创建表
```sql
CREATE TABLE 表名称
(
列名称1 数据类型,
列名称2 数据类型,
列名称3 数据类型,
....
)
```

#### 数据类型
| 数据类型                          | 描述       |
| --------------------------------- | ---------- |
| int(size)                         | 整型       |
| decimal(size, d) numeric(size, d) | 小数       |
| char(size)                        | 字符串     |
| varchar(size)                     | 可变字符串 |
| date(yyyymmdd)                    | 日期       |


### CREATE INDEX 创建索引
- 用户看不到索引，索引只能加快搜索 / 查询
- 在不读取整个表的情况下，使用索引可以更快地查找数据
- 更新一个包含索引的表 比 更新没有索引的表更多时间，因为索引也要更新；所以最好在需要常常被搜索的列或表上创建索引

```sql
允许使用重复的值
CREATE INDEX 索引名
ON 表名 (列名 [ASC, DESC]) 

不能重复，唯一索引
CREATE UNIQUE INDEX 索引名
ON 表名 (列名 [ASC, DESC]) 
```

### CREATE VIEW 创建视图
- 把结果集定义成另一个表，可以更精确地处理我们想要的数据

```sql
CREATE VIEW 视图名 AS
SELECT 列名
FROM 表格名
WHERE condition
```

### ALTER 修改
#### 添加列
```sql
ALTER TABLE 表名
ADD 列名 数据类型
```

####  删除列
```sql
ALTER TABLE 表名
DROP COLUMN 列名
```

#### 修改列的数据类型
```sql
ALTER TABLE 表名
ALTER COLUMN 列名 数据类型
```

### DROP 撤销
#### 删除索引
```sql
ALTER TABLE 表名 
DROP INDEX 索引名
```

#### 删除表 / 数据库
```sql
DROP TABLE 表名

DROP DATABASE 数据库名
```

#### 不删除表，只清空数据
```sql
TRUNCATE TABLE 表名
```

## DML —— 数据操作语言
- SELECT 选取
- UPDATE 更新
- DELETE 删除
- INSERT INTO 插入

### SELECT 语句(选取)

```sql
SELECT 列名称 
FROM 表名称

SELECT * 
FROM 表名称

关键词 DISTINCT 返回不重复行的列数据
SELECT DISTINCT 列名称 
FROM 表名称 
```

#### WHERE 子句(条件)


```sql
SELECT 列名称 
FROM 表名称 
WHERE 列 运算符 值
```

##### 操作符

| 操作符         | 描述                                                         |
| -------------- | ------------------------------------------------------------ |
| =              | 等于                                                         |
| <> 或 !=       | 不等于                                                       |
| >              | 大于                                                         |
| <              | 小于                                                         |
| >=             | 大于等于                                                     |
| <=             | 小于等于                                                     |
| BETWEEN... AND | 在某个范围内，可以是数值、文本或日期；如果在范围外，只需要在BETWEEN前加NOT操作符 |
| LIKE           | 搜索某种模式(使用通配符)                                     |
| AND & OR       | 基于一个以上的条件对记录进行过滤                             |
| IN             | 规定多个值: WHERE 列名称 IN (值1, 值2...)                    |

##### 通配符
- SQL通配符可以替代一个或多个字符，必须与LIKE运算符一起使用

| 通配符                   | 描述                       |
| ------------------------ | -------------------------- |
| %                        | 替代一个或多个字符         |
| _                        | 仅替代一个字符             |
| [charlist]               | 字符列中的任何单一字符     |
| [^charlist]或[!charlist] | 不在字符列中的任何单一字符 |

#### ORDER BY 子句(排序)
- 对结果集进行排序

- ASC 升序 默认
- DESC 降序 

##### 注意

- 如果第一列中有相同值的行或者值为null，后面会以升序排列（如规定了倒序就是倒序）

```sql
SELECT Company, OrderNumber 
FROM Orders 
ORDER BY Company DESC, OrderNumber ASC
```

| Company  | OrderNumber |
| -------- | ----------- |
| W3School | 2356        |
| W3School | 6953        |
| IBM      | 3532        |
| Apple    | 4698        |

### UPDATE 语句(修改)

```sql
UPDATE 表名称 
SET 列名称 = 新值, 新列名称 = 新值
WHERE 列名称 = 某值
```

### DELETE 语句(删除)

```sql
删除表中的行
DELETE FROM 表名称 
WHERE 列名称 = 值

删除所有行
DELECT FROM 表名称
DELECT * FROM 表名称
```

### INSERT INTO 语句(插入)

```sql
INSERT INTO 表名称 
VALUES (值1, 值2,....)

INSERT INTO 表名称 (列1, 列2,...) 
VALUES (值1, 值2,....)
```

