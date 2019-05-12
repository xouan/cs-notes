# mysql环境基本操作
1. 查看MySQL版本、帮助
```
C:\Users\win7>mysql --version
mysql  Ver 14.14 Distrib 5.7.16, for Win64 (x86_64)
C:\Users\win7>mysql --help
mysql  Ver 14.14 Distrib 5.7.16, for Win64 (x86_64)
```
2. 登录MySQL
```
C:\Users\win7>mysql -u root -p
Enter password: ******
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 13
Server version: 5.7.16-log MySQL Community Server (GPL)
```
3. 退出mysql
```
mysql> quit;
Bye

C:\Users\win7>
mysql> exit;
Bye

C:\Users\win7>
```
4. 显示数据库状态
```
mysql> status;
--------------
mysql  Ver 14.14 Distrib 5.7.16, for Win64 (x86_64)

Connection id:          19
Current database:       yiibai
Current user:           root@localhost
SSL:                    Not in use
Using delimiter:        ;
Server version:         5.7.16-log MySQL Community Server (GPL)
Protocol version:       10
Connection:             localhost via TCP/IP
Server characterset:    utf8
Db     characterset:    utf8
Client characterset:    gbk
Conn.  characterset:    gbk
TCP port:               3306
Uptime:                 8 days 15 hours 57 min 16 sec
```
5. 显示数据库数量
```
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mybatis            |
| mysql              |
| performance_schema |
| sakila             |
| sampledb           |
| shop               |
| ssmdemo            |
| sys                |
| world              |
| yiibai             |
+--------------------+
11 rows in set (0.00 sec)
```
6. 使用数据库
```
mysql> use mybatis;
Database changed
```
7. 查询当前使用的数据库名
```
mysql> select database();
+------------+
| database() |
+------------+
| mybatis    |
+------------+
1 row in set (0.00 sec)
```
8. 显示表数量
```
mysql> show tables;
+-------------------+
| Tables_in_mybatis |
+-------------------+
| user              |
+-------------------+
1 row in set (0.00 sec)
```
9. 显示表中所有字段，表结构
```
mysql> desc dept;
+--------+------------------+------+-----+---------+----------------+
| Field  | Type             | Null | Key | Default | Extra          |
+--------+------------------+------+-----+---------+----------------+
| deptno | int(10) unsigned | NO   | PRI | NULL    | auto_increment |
| dname  | varchar(15)      | YES  |     | NULL    |                |
| loc    | varchar(50)      | YES  |     | NULL    |                |
+--------+------------------+------+-----+---------+----------------+
3 rows in set (0.01 sec)
```

# 数据库的创建、删除、
# sql语言及其种类
## DDL(data definition language,数据定义语言)
用来创建或者删除存储数据用的数据库以及数据库中的表等对象

1. CREATE:创建数据库和表等对象
create database <数据库名>
create database shop
create table <表名> (
    <列1> <数据类型> <该列约束>,
    <列2> <数据类型> <该列约束>,
    ...
    <该表的约束>...yi
);
 CREATE TABLE Product
 (product_id char(4) not null,
 product_name varchar(100) not null,
 product_type varchar(32) not null,
 sale_price integer  ,
 purchase_price integer  ,
 regist_date date  ,
 primary key (product_id)
 );

2. DROP：删除数据库和表等对象
drop database <数据库名>
drop table <表名>

3. ALTER:修改数据库和表等对象

## DML(data mamipulation language, 数据操纵语言) （*****）
用来查询或者变更表中的记录

1. SELECT：查询表中的数据
2. INSETT:向表中插入新数据
3. UPDATE：更新表中的数据
4. DELETE:删除表中的数据

## DCL(data control language, 数据控制语言)
用来确认或者取消对数据库中的数据进行的变更。除此之外，还可以对 RDBMS 的用户是否有权限操作数据库中的对象（数据库表等）进行设定。

1. COMMIT：确认对数据库中的数据进行的变更
2. ROLLBACK:取消数据库中的数据进行的变更
3. GRANT：赋予用户操作权限、
4. REVOKE：取消用户的操作权限

# 查询基础
## select语句基础
1. 查询表中指定字段
```
mysql> select dname, loc from dept;
+------------+----------+
| dname      | loc      |
+------------+----------+
| ACCOUNTING | NEW YORK |
| RESEARCH   | DALLAS   |
| SALES      | CHICAGO  |
| OPERATIONS | BOSTON   |
| jfo        | sgresg   |
+------------+----------+
5 rows in set (0.00 sec)
```
2. 查询表中所有字段
```
mysql> select * from dept;
+--------+------------+----------+
| deptno | dname      | loc      |
+--------+------------+----------+
|     10 | ACCOUNTING | NEW YORK |
|     20 | RESEARCH   | DALLAS   |
|     30 | SALES      | CHICAGO  |
|     40 | OPERATIONS | BOSTON   |
|     55 | jfo        | sgresg   |
+--------+------------+----------+
5 rows in set (0.00 sec)
```
2. 为列设定别名
```
mysql> select deptno as id, dname as 姓名 from dept;
+----+------------+
| id | 姓名       |
+----+------------+
| 10 | ACCOUNTING |
| 20 | RESEARCH   |
| 30 | SALES      |
| 40 | OPERATIONS |
| 55 | jfo        |
+----+------------+
5 rows in set (0.00 sec)
```
