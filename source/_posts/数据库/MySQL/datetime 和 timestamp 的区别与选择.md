---
title: MySQL-datetime 和 timestamp 的区别与选择
date: 2020-04-29 18:17:47
categories:
- MySQL
comments: false
---

某次建表时，在选择时间存储类型上有点纠结，需求是：默认数据为当前时间，如果插入 null ，也为当前时间。本身对时间存储类型就不太熟悉，接下来记录一下它们的区别和选择
<!-- more -->



### 区别

|          | datetime                                                   | timestamp                                                  |
| -------- | ---------------------------------------------------------- | ---------------------------------------------------------- |
| 占据字节 | 8 字节                                                     | 4 字节（空间利用率更高）                                   |
| 形式     | yyyy-mm-dd hh:mm:ss                                        | yyyy-mm-dd hh:mm:ss                                        |
| 表示范围 | 1000-01-01 00:00:00.000000' to '9999-12-31 23:59:59.999999 | 1970-01-01 00:00:01.000000' to '2038-01-19 03:14:07.999999 |
| 默认值   | null                                                       | 不为空（not null）,默认值为当前时间（CURRENT_TIMESTAMP）   |
| 时区     | 不会进行时区的检索，存什么拿到的就是什么                   | 以 `utc` 的格式储存， 它会自动检索当前时区并进行转换       |


`timestamp` 翻译为汉语即"时间戳"，它是当前时间到 Unix元年(1970 年 1 月 1 日 0 时 0 分 0 秒)的秒数。

```bash
# 下面都是 MySQL 允许的插入形式，MySQL 会自动处理
2016-10-01 20:48:59
2016#10#01 20/48/59
20161001204859
```



### 选择

1. 需要跨时区，使用 `timestamp` 

2. 如果是想要使用自动插入时间或者自动更新时间功能的，可以使用 `timestamp` 

3. 如果只是想表示年、日期、时间的还可以使用 `year`、 `date`、 `time`，它们分别占据 1、3、3 字节，而`datetime` 就是它们的集合



### 参考文章
[MySQL 中 datetime 和 timestamp 的区别与选择](https://segmentfault.com/a/1190000017393602)