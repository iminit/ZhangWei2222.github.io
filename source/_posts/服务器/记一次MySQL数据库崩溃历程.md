---
title: 记一次MySQL数据库崩溃历程
date: 2020-04-24 10:08:47
categories:
- 服务器
tags:
comments: false
---

上星期在等待毕业论文的批改时，想着把开发毕业项目时收录的问题和解决方法整理成文章。就在自己验证数据库的某个操作时，突然数据库报错打不开，赶紧百度，折腾了一天，最后数据库真的凉了，只能选择重装，抱着应该可以自动找回之前的数据的微弱希望，重装了后发现还是报错，在这个时候突然醒悟了如何解决以及当初自己如何一步步走向深渊。尽管最后还是要重录数据，但是摸索出来了原因后，心里美滋滋。接下来还原一下当时的场景和整理解决方法。

<!-- more -->

## 踩坑记

1. 打开 HediSQL 输入密码，发现

   ```
   Access denied for user 'root'@'ip地址' (using password: YES)
   ```

2. 服务器连接 mySQL ，能够成功

3. 百度之后，在 .my.cnf文件中的  `mysqld`  加 `skip-grant-tables`

4. 除了加字段，还尝试了改密码

5. 重启服务器，重启数据库 `systemctl restart mysqld`

6. 结果报错

   ```
   Job for mysql.service failed because the control process exited with error code.
   See "systemctl status mysql.service" and "journalctl -xe" for details.
   ```

   如果是使用 `mysql -u root -p` 会报

   ```
   Can't connect to local MySQL server through socket '/var/lib/mysql/mysql.sock
   ```

7. 接下来一直围绕着上面两个报错打转，网上的方法要么是内存泄露，但是我的内存没有，要么是改.sock，或者改成tmp.sock 软连接。反复尝试，报错依旧，最后破罐子破摔，重装！

8. 重装后，使用 `systemctl restart mysqld` 重启数据库，这时服务器连接 mySQL 终于没问题了，数据因为前面为了解决报错乱输命令的缘故已删除或损坏，无法还原。但打开HediSQL发现还是出错，（注意：服务器连接mySQL没问题）便打算去阿里云服务器中点击重启，然后重启数据库报错了！这个时候是为何呢？

9. 想起第一次报错的时候，自己想到是不是服务器的锅，重启服务器后再重启数据库，在服务器连接mySQL才出现的 .sock 地狱，之前尽管HediSQL报错，但是服务器是可以连接mySQL的，所以问题出在重启服务器上！

10. 百度‘重启服务器 无法启动 mySQL’，很快[解决办法](https://blog.csdn.net/linuxlsq/article/details/52606479)就搜出来了：**需要删除 mySQL 文件中的 ib_logfile0 和 ib_logfile1 两个文件**



> mySQL 的 innodb 中事务日志：ib_logfile
>
> - 事务日志或称 redo 日志，在 mySQL 中默认以 ib_logfile0,ib_logfile1 名称存在,可以手工修改参数，调节
>   开启几组日志来服务于当前 mySQL 数据库,mySQL 采用顺序，循环写方式，每开启一个事务时，会把一些相关信息记录事务日志中(记录对数据文件数据修改的物理位置或叫做偏移量)；
> - 作用:在系统崩溃重启时，作事务重做；在系统正常时，每次checkpoint时间点，会将之前写入事务
>   应用到数据文件中。

设想，在服务器重启的时候，数据库从原有的事务日志中恢复，但是发生一些异常，比如 bin-log 没写，最后导致主从不一致。**解决办法：干净关闭数据库情况下，删除 mySQL 文件中的 ib_logfile，而后重启数据库，会自行创建该文件。**



## 总结

这次的崩溃真是意想不到，因为之前也试过重启服务器的操作，但是没有出错，令人感到玄学，所以以后还是不要随意重启服务器为妙。HediSQL的报错最后解决的也比较玄学，在删除两个文件后，又可以连接了。不确定是不是因为之前尝试过在 my.conf [mysqld]中添加`skip-name-resolve`，配置账号远程权限`grant all privileges on *.* to 'mysql'@'%' identified by 'passwd' with grant option; flush privileges;` 解决思路应该是这样子。



也引申出一个很重要的问题：为何当初没做数据库备份！！！！如果备份了，就不会在重不重装上犹豫不决，为了不重装不重录数据，一直死磕...幸亏这次是数据库中没有太多逻辑上的 SQL语句，大部分是表、试图、简单的存储过程和定时事件，不然真的会猛虎落泪 ><。所以在重录数据库后，赶紧做了一次定时数据库备份操作，将在下一篇文章中阐述，如何定时备份数据库，定时删除多余数据库数据以及如何还原数据库。