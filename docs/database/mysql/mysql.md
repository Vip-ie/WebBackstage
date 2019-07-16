# Mysql

## 1. Mysql基础

### 1.1 Mysql简单操作

#### 1.1.0 登录MySQL
```
mysql -u + user -p + password;
```

#### 1.1.1 创建用户
```
mysql>create user 'leva'@'%' identified by 'leva123';
```

#### 1.1.2 给用户赋予权限
```
mysql>grant all on *.* to 'leva'@'%';
```

#### 1.1.3 使更改立即生效
```
mysql>flush privileges;
```

#### 1.1.4 退出
```
mysql>\q
mysql>exit
```

#### 1.1.5 查看当前用户
```
select user();
```

#### 1.1.6 查看当前数据库
```
select database（);
```

#### 1.1.7 设置数据库编码为utf8
```
mysql> alter database zy character set utf8;
Query OK, 1 row affected, 1 warning (0.03 sec)

查看数据库编码
mysql> show create database zy\G
*************************** 1. row ***************************
       Database: zy
Create Database: CREATE DATABASE `zy` /*!40100 DEFAULT CHARACTER SET utf8 */
1 row in set (0.00 sec)
```

### 2.1 创建与删除数据库

#### 2.1.1 创建数据库 语法
create database [in not exists] 重复创建会报错，所以可以加上if not exists
**注意:** SQL语句必须以分号结尾。

#### 2.1.2 创建数据库
```
mysql> create database mydb;
Query OK, 1 row affected (0.06 sec)
```

#### 2.1.3 查看数据库方法
```
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mydb               |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
6 rows in set (0.00 sec)
```

#### 2.1.4 删除数据库 语法
`drop database [in exists] dbname;`如果不知道数据库，是否存在记得加if exists。

```
mysql> drop database mydb;
Query OK, 0 rows affected (0.11 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.01 sec)
```

#### 2.1.5 Mysql数据库使用
* 查看数据库
select database();
```
mysql> select database();
+------------+
| database() |
+------------+
| NULL       |    NULL表示没有进入任何数据库，其他名字表示数据名
+------------+
1 row in set (0.00 sec)
```

* 进入数据库
use dbname; 注意：数据库创建成功，并没有直接使用
```
mysql> use college
Database changed

mysql> select database();
+------------+
| database() |
+------------+
| college    |
+------------+
1 row in set (0.00 sec)
```

### 1.3 创建和删除数据库表

#### 1.3.1 MySQL创建表
创建表 语法

create table [if not exists] table_name(

column_name data_type,

);

```
mysql> create table if not exists student(
    -> id int,
    -> name varchar(10)
    -> );
Query OK, 0 rows affected (0.06 sec)
```

#### 1.3.2 查看数据库结构 语法
* describe tb_name;
```
mysql> desc student;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  |     | NULL    |       |
| name  | varchar(10) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
```

* show create table tb_name;(\G)查看表结构使用了那些命令
```
mysql> show create table student\G
*************************** 1. row ***************************
       Table: student
Create Table: CREATE TABLE `student` (
  `id` int(11) DEFAULT NULL,
  `name` varchar(10) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
1 row in set (0.00 sec)
```

=======
#### 1.3.3 查看数据库有那些表语法

show tables;
```
mysql> show tables;
+----------------+
| Tables_in_test |
+----------------+
| student        |
+----------------+
1 row in set (0.00 sec)
```

#### 1.3.4 删除表 语法
drop table tablename;
```
mysql> drop table student;
Query OK, 0 rows affected (0.03 sec)
```

### 1.4 数据库表增、删、改、查

#### 1.4.1 数据库表 插入数据

* insert 语法一
插入单条数据
```
mysql> insert into test (id,name) value(1,'Rave');
Query OK, 1 row affected (0.03 sec)
```
插入多条数据
```
mysql> insert into test(id,name) value(2,'leva'),(3,'zlk');
Query OK, 2 rows affected (0.02 sec)
Records: 2  Duplicates: 0  Warnings: 0
```

* insert 语法二
insert [into] tablename set col_name = {expr|default}
```
mysql> insert into test set id = 4,name = 'akai';
Query OK, 1 row affected (0.04 sec)
```

#### 1.4.2 数据库表 查询数据

select 查询 语法
查询表内所有数据 * 表示所有内容`select * from table_name [where];`
```
mysql> select * from test;
+------+------+
| id   | name |
+------+------+
|    1 | Rave |
|    2 | leva |
|    3 | zlk  |
|    4 | akai |
+------+------+
4 rows in set (0.00 sec)
```
只查询表内指定字段内容
```
mysql> select name from test where id >2;
+------+
| name |
+------+
| zlk  |
| akai |
+------+
2 rows in set (0.00 sec)
```

#### 1.4.3 数据库表 更改数据

select 查询语法`update tablename set col_name1={expr1|default} [where]`
```
mysql> update test set name = '不动' where id = 3;
Query OK, 1 row affected (0.06 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```

#### 1.4.4 数据库表 删除数据

delete 删除语法`delete from table_name where where_conditon;`

**注意:** 一定要写where条件，不然会删除全部数据。
```
mysql> delete from test where id = 1;
Query OK, 1 row affected (0.04 sec)
```

#### 1.4.5 表结构增、删、改、查
* **表结构 增 add**
在表结构内第一行增加一个字段
```
mysql> create table tb1(
    -> id int,
    -> name char(4)
    -> );
Query OK, 0 rows affected (0.08 sec)

mysql> alter table tb1
    -> add age int first;    ##插入列内第一行数据
Query OK, 0 rows affected (0.07 sec)
Records: 0  Duplicates: 0  Warnings: 0
```

在表结构内插入到指定位置指定字段
```
mysql> alter table tb1
    -> add age int after id;     ##after表示插入到指定字段后面
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc tb1;
+-------+---------+------+-----+---------+-------+
| Field | Type    | Null | Key | Default | Extra |
+-------+---------+------+-----+---------+-------+
| id    | int(11) | YES  |     | NULL    |       |
| age   | int(11) | YES  |     | NULL    |       |
| name  | char(4) | YES  |     | NULL    |       |
+-------+---------+------+-----+---------+-------+
3 rows in set (0.00 sec)
```

* **表结构 删 drop**
```
+-------+---------+------+-----+---------+-------+
| Field | Type    | Null | Key | Default | Extra |
+-------+---------+------+-----+---------+-------+
| id    | int(11) | YES  |     | NULL    |       |
| age   | int(11) | YES  |     | NULL    |       |
| name  | char(4) | YES  |     | NULL    |       |
+-------+---------+------+-----+---------+-------+
3 rows in set (0.00 sec)

mysql> alter table tb1
    -> drop age;
Query OK, 0 rows affected (0.07 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc tb1;
+-------+---------+------+-----+---------+-------+
| Field | Type    | Null | Key | Default | Extra |
+-------+---------+------+-----+---------+-------+
| id    | int(11) | YES  |     | NULL    |       |
| name  | char(4) | YES  |     | NULL    |       |
+-------+---------+------+-----+---------+-------+
2 rows in set (0.01 sec)
```

* **表结构 改** （数据类型方法 modify 字段名称方法 change 改表名rename）
修改表结构内数据类型方法
```
mysql> alter table tb1
    -> modify name varchar(10);  ## 改表结构数据类型
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc tb1;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  |     | NULL    |       |
| name  | varchar(10) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
```

改表结构字段名称
```
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  |     | NULL    |       |
| name  | varchar(10) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)

mysql> alter table tb1
    -> change name sex char(4);  ##修改字段名称以及属性类型
Query OK, 0 rows affected (0.06 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc tb1;
+-------+---------+------+-----+---------+-------+
| Field | Type    | Null | Key | Default | Extra |
+-------+---------+------+-----+---------+-------+
| id    | int(11) | YES  |     | NULL    |       |
| sex   | char(4) | YES  |     | NULL    |       |
+-------+---------+------+-----+---------+-------+
2 rows in set (0.00 sec)
```

改表名方法
```
+----------------+
| Tables_in_mydb |
+----------------+
| tb2            |
+----------------+
6 rows in set (0.00 sec)

mysql> alter table tb2
    -> rename tb1;    ## 表改名
Query OK, 0 rows affected (0.06 sec)

mysql> show tables;
+----------------+
| Tables_in_mydb |
+----------------+
| tb1            |
+----------------+
6 rows in set (0.00 sec)
```

* **表结构 查**

show create table tablename
```
mysql> desc tb1; ##看看表结构方法
+-------+---------+------+-----+---------+-------+
| Field | Type    | Null | Key | Default | Extra |
+-------+---------+------+-----+---------+-------+
| id    | int(11) | YES  |     | NULL    |       |
| sex   | char(4) | YES  |     | NULL    |       |
+-------+---------+------+-----+---------+-------+
2 rows in set (0.00 sec)

mysql> show create table tb1\G  ##查看表使用了那些命令
*************************** 1. row ***************************
       Table: tb1
Create Table: CREATE TABLE `tb1` (
  `id` int(11) DEFAULT NULL,
  `sex` char(4) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
1 row in set (0.00 sec)
```

### 1.5 Mysql数据类型
常用的4种：整型、浮点型、日期类型、字符
```
mysql> create table st(
    -> id int,                    ## 整型
    -> name varchar(20),          ## 指定长度，最多65535个字符
    -> sex char(4),               ## 指定长度，最多255个字符，定长
    -> price double(4,2),         ## 双精度浮点型，m总个数，d小数为
    -> detail text,               ## 可变长度，最多65535个字符
    -> dates datetime,            ## 日期时间类型 YYYY-MM-DD HH:MM:SS
    -> ping enum('好评','差评')    ## 枚举，在给出的value中选择
    -> );
```

```
insert into table vale(1,‘裤子’，‘男’，20.0，‘这条裤子超好！！！’，now(),'好评');
```

### 1.6 练习题

#### 1.6.1 建一张学生表包含（id、姓名、年龄、性别）
```
mysql> create table student(
    -> id int,
    -> name varchar(10),
    -> age int,
    -> sex varchar(10)
    -> );
Query OK, 0 rows affected (0.09 sec)
```

#### 1.6.2 查看表结构
```
mysql> desc student;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  |     | NULL    |       |
| name  | varchar(10) | YES  |     | NULL    |       |
| age   | int(11)     | YES  |     | NULL    |       |
| sex   | varchar(10) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
4 rows in set (0.00 sec)
```

#### 1.6.3 增加四条数据
```
mysql> insert into student values(1,'zlk',25,'男'),(2,'rave',23,'女'),(3,'leva',29,'女'),(4,'one',25,'女');
Query OK, 4 rows affected (0.03 sec)
Records: 4  Duplicates: 0  Warnings: 0
```

#### 1.6.4 查询所有数据
```
mysql> select * from student;
+------+------+------+------+
| id   | name | age  | sex  |
+------+------+------+------+
|    1 | zlk  |   25 | 男   |
|    2 | rave |   23 | 女   |
|    3 | leva |   29 | 女   |
|    4 | one  |   25 | 女   |
+------+------+------+------+
4 rows in set (0.00 sec)
```

#### 1.6.5 删除id=3的数据
```
mysql> delete from student where id = 3;
Query OK, 1 row affected (0.09 sec)mysql> 

mysql>select * from student;
+------+------+------+------+
| id   | name | age  | sex  |
+------+------+------+------+
|    1 | zlk  |   25 | 男   |
|    2 | rave |   23 | 女   |
|    4 | one  |   25 | 女   |
+------+------+------+------+
3 rows in set (0.00 sec)
```

#### 1.6.6 将性别女的，修改为男
```
mysql> update student set sex = '男' where id = 2;
Query OK, 1 row affected (0.10 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> update student set sex = '男' where id = 4;
Query OK, 0 rows affected (0.00 sec)
Rows matched: 0  Changed: 0  Warnings: 0

mysql> select * from student;
+------+------+------+------+
| id   | name | age  | sex  |
+------+------+------+------+
|    1 | zlk  |   25 | 男   |
|    2 | rave |   23 | 男   |
|    4 | one  |   25 | 男   |
+------+------+------+------+
3 rows in set (0.01 sec)
```

#### 1.6.7 简单说明用户，数据库，表与数据的关系

## 2. 表约束

### 2.1 非空约束
**有非空约束的字段，insert的时候，必须添加内容**

注意：空字符不等于null

#### 2.1.1`desc`查看表结构信息
```
desc tb1;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  |     | NULL    |       |
| name  | varchar(10) | NO   |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
```

手动，添加非空约束方法（必须这个字段，没有null值）
```
mysql> alter table tb1 
    -> modify id int not null;
Query OK, 0 rows affected (0.09 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc tb1;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | NO   |     | NULL    |       |
| name  | varchar(10) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
```

#### 2.1.2 取消非空约束
```
mysql> alter table tb1
    -> modify name varchar(10);
Query OK, 0 rows affected (0.09 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc tb1;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  |     | NULL    |       |
| name  | varchar(10) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
```

### 2.2 唯一约束

### 2.3 主键约束

### 2.4 自增长

### 2.5 默认约束

### 2.6 外键约束

## 3. 表关关系

### 3.1 一对一关系

### 3.2 一对多关系

### 3.3 多对多关系

### 3.4 练习题

## 4. 表查询

### 4.1 表单查询

### 4.2 子表查询

### 4.3 关联查询

### 4.4 练习题

## 5. 事务

## 6. python 操作mysql
