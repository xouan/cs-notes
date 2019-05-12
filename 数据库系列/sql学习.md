# 一、 数据库环境搭建与基本操作

查看数据库版本、登录数据库、退出数据库、显示数据库状态、显示数据库数量、使用数据库、显示表数量、查询当前使用的数据库名

## 1 mysql环境基本操作

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

## 显示数据库与表

ALTER  USER  'root'  IDENTIFIED  WITH  mysql_native_password  BY  '123456';



### 函数

1算数函数

| -          | -              |
| ---------- | -------------- |
| 函数       | 功能           |
| +，-，*，/ | 加，减，乘，除 |
| abs        | 绝对值         |
| mod        | 取余数         |
| round      | 四舍五入       |
|            |                |

```sql

mysql> select * from sample_math;
+----------+------+------+
| m        | n    | p    |
+----------+------+------+
| 500.000  |    0 | NULL |
| -180.000 |    0 | NULL |
| NULL     | NULL | NULL |
| NULL     |    7 |    3 |
| NULL     |    5 |    2 |
| NULL     |    4 | NULL |
| 8.000    | NULL |    3 |
| 2.270    |    1 | NULL |
| 5.555    |    2 | NULL |
| NULL     |    1 | NULL |
| 8.760    | NULL | NULL |
+----------+------+------+
mysql> select m, abs(m) as abs_col from sample_math;
+----------+---------+
| m        | abs_col |
+----------+---------+
| 500.000  | 500.000 |
| -180.000 | 180.000 |
| NULL     | NULL    |
| NULL     | NULL    |
| NULL     | NULL    |
| NULL     | NULL    |
| 8.000    | 8.000   |
| 2.270    | 2.270   |
| 5.555    | 5.555   |
| NULL     | NULL    |
| 8.760    | 8.760   |
+----------+---------+

mysql> select n, p mod(n, p) as mod_col from sample_math;
1241 - Operand should contain 1 column(s)
mysql> select n, p, mod(n, p) as mod_col from sample_math;
+------+------+---------+
| n    | p    | mod_col |
+------+------+---------+
|    0 | NULL | NULL    |
|    0 | NULL | NULL    |
| NULL | NULL | NULL    |
|    7 |    3 |       1 |
|    5 |    2 |       1 |
|    4 | NULL | NULL    |
| NULL |    3 | NULL    |
|    1 | NULL | NULL    |
|    2 | NULL | NULL    |
|    1 | NULL | NULL    |
| NULL | NULL | NULL    |
+------+------+---------+

```

2字符串函数

| -              | -          |
| -------------- | ---------- |
| 函数           | 功能       |
| concat         | 字符串拼接 |
| length(字符串) | 字符串长度 |
| lower(str)     | 字符串小写 |
| upper(str)     | 字符串大写 |
|                |            |



3日期函数

| -                 | -            |
| ----------------- | ------------ |
| 函数              | 功能         |
| current_date      | 当前日期     |
| current_time      | 当前时间     |
| current_timestamp | 当前日期时间 |
| extract           | 截取日期元素 |

```
mysql> select current_date;
+--------------+
| current_date |
+--------------+
| 2019-01-25   |
+--------------+
mysql> select current_time;
+--------------+
| current_time |
+--------------+
| 11:18:14     |
+--------------+
mysql> select current_timestamp;
+---------------------+
| current_timestamp   |
+---------------------+
| 2019-01-25 11:18:43 |
+---------------------+
1 row in set (0.02 sec)
mysql> select current_timestamp,
    -> extract(year from current_timestamp) as year,
    -> extract(month from current_timestamp) as month,
    -> extract(day from current_timestamp) as day,
    -> extract(hour from current_timestamp) as hour,
    -> extract(minute from current_timestamp) as minute,
    -> extract(second from current_timestamp) as second;
+---------------------+------+-------+-----+------+--------+--------+
| current_timestamp   | year | month | day | hour | minute | second |
+---------------------+------+-------+-----+------+--------+--------+
| 2019-01-25 11:22:39 | 2019 |     1 |  25 |   11 |     22 |     39 |
+---------------------+------+-------+-----+------+--------+--------+
```



4转换函数

5聚合函数



