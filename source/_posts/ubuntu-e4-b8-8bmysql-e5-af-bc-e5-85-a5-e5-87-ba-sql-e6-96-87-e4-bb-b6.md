---
title: ubuntu 下mysql导入出.sql文件
url: 3362.html
id: 3362
categories:
  - 运维
date: 2016-03-31 17:39:42
tags:
---

1.导出整个数据库 mysqldump -u 用户名 -p 数据库名 > 导出的文件名 mysqldump -u wcnc -p smgp\_apps\_wcnc > wcnc.sql 2.导出一个表 mysqldump -u 用户名 -p 数据库名 表名> 导出的文件名 mysqldump -u wcnc -p smgp\_apps\_wcnc users> wcnc\_users.sql 3.导出一个数据库结构 mysqldump -u wcnc -p -d –add-drop-table smgp\_apps\_wcnc >d:\\wcnc\_db.sql -d 没有数据 –add-drop-table 在每个create语句之前增加一个drop table 4.导入数据库 常用source 命令 进入mysql数据库控制台， 如mysql -u root -p mysql>use 数据库 然后使用source命令，后面参数为脚本文件(如这里用到的.sql) //mysql>source /home/pt/test.sql //或直接导入命令为： mysql -h localhost -u root -p temp [Ubuntu](http://www.linuxidc.com/topicnews.aspx?tid=2 "Ubuntu") 下首先建好数据库，如temp mysql -h localhost -u root -p temp</home/pt/test.sql 会提示你输入密码，输入就OK。 2. 导入数据: 语法是 mysqldump –opt 库名 < c:\\data.sql 例子是 mysqldump -hlocalhost -uroot -p123456 zf 或者 mysqlimport -hlocalhost -u root -p123456 &lt; c:\\data.sql 3. 将文本数据导入数据库: use 库名; load data local infile “文件名” into table 表名; 第六招 创建一个数据库表 CREATE TABLE MYTABLE (name VARCHAR(20), sex CHAR(1)); 第七招 往表中加入记录 insert into 表名 values (”1″,”2″); 第八招 用文本方式将数据装入数据库表中 LOAD DATA LOCAL INFILE “c:/data.sql” INTO TABLE 表名; 第九招 导入.sql文件命令（例如c:/data.sql） use database; source c:/data.sql 第十招 更新表中数据 update MYTABLE set sex=”f” where name=’hyq’; 第十一招 修复表 repair 表名 第十二招 查看表的大小 show 表名 status 第十三招 修改密码 mysqladmin -u用户名 -p旧密码 password “新密码” 第十四招 修改表结构 ALTER TABLE t1 MODIFY b BIGINT NOT NULL; 第十四招 退出MYSQL命令 exit or quit（回车）