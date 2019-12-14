---
title: SQL-函数
date: 2019-08-22 10:59:37
categories:
- 数据库
tags:
- SQL
comments: false
---
## 前言
整理函数表，方便查询~

<!-- more -->

## 函数表

| 函数名                          | 语法                                                         | 描述                   |
| ------------------------------- | ------------------------------------------------------------ | ---------------------- |
| AVG(列名)                       | SELECT AVG(column_name) FROM table_name                      | 平均值                 |
| COUNT(列名)                     | SELECT COUNT(*) FROM table_name<br />SELECT COUNT(column_name) FROM table_name<br />SELECT COUNT(DISTINCT column_name) FROM table_name | 匹配条件的行数         |
| FIRST(列名)                     | SELECT FIRST(column_name) FROM table_name                    | 第一条记录             |
| LAST(列名)                      | SELECT LAST(column_name) FROM table_name                     | 最后一条记录           |
| MAX(列名)                       | SELECT MAX(column_name) FROM table_name                      | 最大值                 |
| MIN(列名)                       | SELECT MIN(column_name) FROM table_name                      | 最小值                 |
| SUM(列名)                       | SELECT SUM(column_name) FROM table_name                      | 总数                   |
| UCASE(列名)                     | SELECT UCASE(column_name) FROM table_name                    | 大写                   |
| LCASE(列名)                     | SELECT LCASE(column_name) FROM table_name                    | 小写                   |
| MID(列名，开始位置，返回字符数) | SELECT MID(column_name,start[,length]) FROM table_name       | 从文本字段中提取字符   |
| LEN(列名)                       | SELECT LEN(column_name) FROM table_name                      | 文本字段中值的长度     |
| ROUND(列名，指定小数位数)       | SELECT ROUND(column_name,decimals) FROM table_name           | 返回指定小树位数的数值 |
| NOW()<br />GETDATE()            | SELECT NOW() FROM table_name //MySql<br />SELECT GETDATE() FROM table_name //Sql Server | 当前日期和时间         |
| FORMAT(列名)                    |                                                              |                        |




## NULL 函数
如果某列是可选时，为空的数据值为NULL。但是NULL不等于0，我们并不好处理。所以可以使用NULL函数，转换成数字，再来处理。不同的数据库中NULL函数的表示不同，但用法相同
```sql
SELECT ProductName,UnitPrice*(UnitsInStock+ISNULL(UnitsOnOrder,0))
FROM Products
```

- SQL Server / MS Access —— ISNULL()
- Oracle —— NVL()
- MySQL —— IFNULL()， COALESCE()

## Date 函数
注意以下查询是得不到结果的

| OrderId | ProductName | OrderDate           |
| :------ | :---------- | :------------------ |
| 1       | computer    | 2008-12-26 16:23:55 |

```sql
SELECT * FROM Orders WHERE OrderDate='2008-12-26'
```

因为查询中不含有时间部分的日期