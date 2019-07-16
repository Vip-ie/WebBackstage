# Mysql

## 1. Mysql基础

### 1.1 登录MySQL
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
##### 2.1.5.1 查看数据库
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

##### 2.1.5.2 进入数据库
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

## 2. 表约束

## 3. 表关关系

## 4. 表查询

## 5. 事务

## 6. python 操作mysql
