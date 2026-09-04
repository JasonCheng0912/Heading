
<!-- TOC -->
  * [1.创建删除数据库](#1创建删除数据库)
    * [创建数据库](#创建数据库)
    * [查看所有数据库](#查看所有数据库)
    * [删除数据库](#删除数据库)
    * [选择数据库](#选择数据库)
    * [创建一个名为cjs_test的数据库；](#创建一个名为cjs_test的数据库)
    * [选择该数据库](#选择该数据库)
  * [2. 数据类型](#2-数据类型)
    * [整数类型](#整数类型)
    * [浮点类型](#浮点类型)
    * [字符类型](#字符类型)
    * [时间类型](#时间类型)
  * [3. 创建表与删除表](#3-创建表与删除表)
    * [创建表](#创建表)
    * [eg.创建一个employees表 包含雇员id,名字，雇员薪水](#eg创建一个employees表-包含雇员id名字雇员薪水)
    * [查看表](#查看表)
    * [删除表](#删除表)
    * [eg.删除employees表](#eg删除employees表)
  * [4.修改表](#4修改表)
    * [修改表](#修改表)
    * [eg.将employees表名修改为emp](#eg将employees表名修改为emp)
  * [5.修改列名](#5修改列名)
    * [eg.将emp表中的employee_name修改为name](#eg将emp表中的employee_name修改为name)
  * [6.修改列类型](#6修改列类型)
    * [eg.将emp表中 name的长度指定为40](#eg将emp表中-name的长度指定为40)
  * [7.修改新列](#7修改新列)
    * [eg.将emp表添加佣金列，列名为commission_pct](#eg将emp表添加佣金列列名为commission_pct)
  * [8.添加新列](#8添加新列)
    * [eg.删除emp表 佣金列 commission_pct](#eg删除emp表-佣金列-commission_pct)
  * [9.添加数据](#9添加数据)
    * [9.1 选择插入](#91-选择插入-)
    * [eg.向departments表中添加一条数据，部门名称为market，工作地点id为1](#eg向departments表中添加一条数据部门名称为market工作地点id为1)
    * [9.2 完全插入](#92-完全插入-)
    * [注：如果主键是自动增长，需要使用default或者null或者0占位。](#注如果主键是自动增长需要使用default或者null或者0占位)
    * [eg1.向departments表中添加一条数据，部门名称为development，工作地点id为2.使用default占位](#eg1向departments表中添加一条数据部门名称为development工作地点id为2使用default占位)
    * [eg2.向departments表中添加一条数据，部门名称为human，工作地点id为3.使用null占位](#eg2向departments表中添加一条数据部门名称为human工作地点id为3使用null占位)
    * [eg3.向departments表中添加一条数据，部门名称为teaching，工作地点id为4.使用0占位](#eg3向departments表中添加一条数据部门名称为teaching工作地点id为4使用0占位)
<!-- TOC -->



















## 1.创建删除数据库
### 创建数据库
```
create databese test default character set utf8;
```


### 查看所有数据库
```
show datebases;
```
### 删除数据库
```
drop databas 数据库名;
```
### 选择数据库
```
use 数据库;
```

###  创建一个名为cjs_test的数据库；
```
create datebase cjs_test dafault character set utf8;
```
### 选择该数据库
```
use cjs_test
```

## 2. 数据类型
### 整数类型

| Mysql数据类型    | 含义  | 范围       |
|--------------|-----|----------|
| tinyint(m)   | 1字节 | -128~127 |
| smallint(m)  | 2个  |          |
| mediumint(m) | 3个  |          |
| int(m)       | 4 个 |          |
| bigint(m)    | 8 个 |          |


- int(m) 中的 m 指的是该列数据的显示宽度
（即展示时占用的最小字符位数），
-  而不是存储大小（ int 永远占 4 个字节，取值范围固定）  


- 这个 m 只有在字段指定了 ZEROFILL（零填充）时才有实际视觉效果。

- 举例：int(4) zerofill
- 存入值 5：实际存储的是数字 5，但查询显示时会变成 0005（补足 4 位）。
- 存入值 12345：实际存储的是 12345，显示时不会被截断，依然显示 12345（因为其位数超过了显示宽度，优先保证数据完整）。

### 浮点类型

 

| Mysql数据类型   | 含义                        | 说明 |
|-------------|---------------------------|----|
| float(m,d)  | 单精度浮点型  |8位精度(4字节)m总位数，d小数位    |
| double(m,d) | 双精度浮点型  |16位精度(4字节)m总位数，d小数位    |
 
-  m（精度）：代表总共能存多少位数字（整数部分 + 小数部分的总位数）。
-  d（标度）：代表小数点后面保留多少位数字。

### 字符类型

| Mysql数据类型  | 含义   | 范围            |
|------------|------|---------------|
| char(n)    | 固定长度 | 最多255个字符      |
| tinytext   | 可变长度 | 最多255个字符      |
| varchar(n) | 可变长度 | 最多65535个字符    |
| text       | 可变长度 | 最多65535个字符    |
| mediumtext | 可变长度 | 最多2的24次方-1个字符 |
| longtext   | 可变长度 | 最多2的32次方-1个字符 |
 
### 时间类型

| Mysql数据类型 | 含义                       | 说明           |
|-----------|--------------------------|--------------|
| date      | 日期 YYYY-MM-DD            |              |
| time      | 时间 HH:-MM-SS             |              |
| datetime  | 日期时间 YYYY-MM-DD HH:MM:SS | 不支持跨时区       |
| temestamp | 时间戳:YYYYMMMDD HHMMSS     | 支持跨时区 |

 

## 3. 创建表与删除表
### 创建表
```
create table 表名(列名1 类型1,列名2 类型2);
```
### eg.创建一个employees表 包含雇员id,名字，雇员薪水
```
create table employees(employee_id int,employee_name varchar(10),salary float(8,2))

```

### 查看表
```
show tables;
```


### 删除表
```
DROP TABLE 表名;
```
### eg.删除employees表

```
drop table employees;
```


## 4.修改表
### 修改表
```commandline
ALTER TABLE 旧表名 RENAME 新表名;
```
### eg.将employees表名修改为emp
```commandline
alter table employees rename emp；
```

## 5.修改列名

```commandline
ALTER TABLE  表名 CHANGE COLUMN 旧列名 新列名 类型;
```
### eg.将emp表中的employee_name修改为name
```commandline
alter table emp change column employee_name name varchar(20);
```

## 6.修改列类型

```commandline
ALTER TABLE  表名 MODIFY 列名 新类型;
```
### eg.将emp表中 name的长度指定为40
```commandline
alter table emp modify name varchar(40);
```


## 7.修改新列

```commandline
ALTER TABLE  表名 ADD COLUMN 新列名 类型;
```
### eg.将emp表添加佣金列，列名为commission_pct
```commandline
alter table emp add column commission_pct  float(4,2);
```


## 8.添加新列

```commandline
ALTER TABLE  表名 DROP COLUMN  列名; 
```
### eg.删除emp表 佣金列 commission_pct
```commandline
alter table emp drop column commission_pct ;
```

## 9.添加数据
### 9.1 选择插入 
```commandline
insert into 表名(列名1,列名2,列名3......) values(值1,值2,值3......);
```

### eg.向departments表中添加一条数据，部门名称为market，工作地点id为1
```commandline
insert into departments(department_name,location_id) values("market",1);
```
 
### 9.2 完全插入 
```commandline
insert into 表名 values(值1,值2,值3......);
```
### 注：如果主键是自动增长，需要使用default或者null或者0占位。
### eg1.向departments表中添加一条数据，部门名称为development，工作地点id为2.使用default占位

```commandline
insert into departments values(default,"development",2);
```
### eg2.向departments表中添加一条数据，部门名称为human，工作地点id为3.使用null占位

```commandline
insert into departments values(null,"human",3);
```
### eg3.向departments表中添加一条数据，部门名称为teaching，工作地点id为4.使用0占位

```commandline
insert into departments values(0,"teaching",4);
```