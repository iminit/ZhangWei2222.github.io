---
title: MySQL-delimiter 命令的作用
date: 2020-04-27 11:32:47
categories:
- MySQL
comments: false
---

在学习游标的过程中，发现有个教程会写 delimiter 命令，尝试过不写，有时会无法建立存储过程或函数。接下来研究一下 delimiter 命令的作用

<!-- more -->

delimiter 命令的作用：设置命令段的结束符号，即遇到这个所设置的结束符号后，按回车，则命令段就可以执行了。

通常默认情况下，命令的结束符号是分号（;），但是在存储过程中，过程体内可能会包含分号（;），因此需要将命令结束符号替换成其他的字符，如$$、//等，存储过程创建完成后，可以将命令段的结束符号重新设为分号。

> 语法：delimiter 命令段结束符



```mysql
-- 定义语法结束符号
delimiter //
-- 创建一个名称为 p 的存储过程
create procedure p()
BEGIN
	declare mcId int;
	declare flag int default 0;
	-- 声明游标
	declare mc cursor for SELECT id FROM `orders` WHERE STATUS = -1 AND  NOW() >= (orders.startDate + INTERVAL days day);
	declare continue handler for not found set flag = 1;
	-- 打开游标
	open mc;
	-- 循环
	lp:loop 
	
	-- 获取结果
	fetch mc into mcId;
	if flag=1 then -- 当无法fetch会触发handler continue
	  leave lp;
	end if;
	-- 这里是为了显示获取结果
	CALL set_orderStatus(mcId, '0');
	-- 结束循环
	end loop;
	-- 关闭游标
	close mc;
END //
delimiter ;

call p();
```

