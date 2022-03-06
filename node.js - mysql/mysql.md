## mysql

- show database; 显示数据库
- show databases; -- 显示所有数据库
- use 数据库名称 切换mysql[数据库]()
- show tables 查看当前数据库中的表
- select * from 表名; 查询表中的所有列
- select id,name,age * from 表名; 查询id,name,age 字段的数据
- select * from students WHERE id="3"; 查询id为3的行(用到WHERE)
- select * from students WHERE id="3" and age="20"; 查询id为3,age为20的行(用到WHERE，and可以换成or并且后面可以跟多个or)
- select * from students where name like "%A%";查询name包含A的数据
- select * from students where name like "%A";查询name以A结尾的数据
- select * from students where name like "A%";查询name以A开头的数据
- select * from students where age>=18 and age<=25;查询age>=18和小于等于25的数据
- create database 数据库名称； 创建数据库
- English: Query OK, 0 rows affected (0.05 sec) 查询OK，0行受影响（0.05秒）
- delete from students where id=1;删除id=1的数据
- insert into students values(1,"AHao4",20,19990909);增加数据
- insert into students(id,name,age,birthday)values(1,"AHao",20,20000809);增加数据
- select * from students limit 3 offset 2;显示从第2条开始的3条数据
- drop database 数据库名称;删库跑路
- ALTER user 'root'@'localhost' IDENTIFIED BY '新密码' 改密码

