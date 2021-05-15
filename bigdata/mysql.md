windows mysql root密码：alex123

alex: alex123
### 修改用户可以远程登录
```mysql
select user, host from user;
update user set host="%" where user="root";
flush privileges;
```
### 重命名数据库
```bash
 mysqldump -u root -p -R gmal > /opt/module/db_log/gmal.sql;
 mysqladmin -u root -p create gmall
 mysql -u root -p gmall < /opt/module/db_log/gmal.sql  #会出现问题
```
在workbench里，新的数据库上将sql脚本上加上`SET GLOBAL log_bin_trust_function_creators = 1;`语句再执行

### 卸载mysql
1) `dpkg --list|grep mysql`
```bash
sudo apt purge mysql-server mysql-common mysql-client mysql-community-client  mysql-community-client-core mysql-community-client-plugins mysql-community-server mysql-community-server-core mysql-apt-config
```
2) `sudo apt autoremove`
3) `sudo apt autoclean`
# 1. [Ubuntu 安装mysql](https://www.zhihu.com/search?type=content&q=mysql%20ubuntu)
### 方法1：从ubuntu仓库中安装
```bash
sudo apt update
sudo apt install mysql-server -y
```
### 方法2: 使用官方仓库安装 MySQL
```bash
curl -OL https://dev.mysql.com/get/mysql-apt-config_0.8.12-1_all.deb
sudo dpkg -i mysql-apt-config*
sudo apt update
sudo apt install mysql-server -y
```
### 验证是否正确安装成功
```bash
sudo systemctl status mysql.service # 直接使用mysql也行
```
### 启动mysql服务
```bash
sudo systemctl start mysql.service  # 直接使用mysql也行
```
### 配置/保护 MySQL(可选)
```bash
sudo mysql_secure_installation
```
### 连接操作
```bash
mysql -h host_name -u user -p
```
# 2. 常用命令
|命令|功能|
|----|----|
|操作数据库|
|`show databases;`|显示数据库|
|`use database`|使用数据库|
|`create database name`|创建数据库|
|`select database();`|显示所用的数据库|
|`drop database <name>；`|删除数据库|
|`select user,host from mysql.user;`|查看用户信息|
|操作表|
|`show tables;`||
|`create table tablename(***);`|建表|
|`describe/desc tablename;`|查看表内容|
|`load data local infile '/path/file.txt' into table pet;`|从文件中导入|
|`insert into pet values ();`|自己创建|
|`drop TABLE table_name;`|删除表|
|`update table_name set field1=new-value1,**;`|修改表内数据|
|`delete from table_name [where **];`|删除一行数据|
|`alter table table_name drop field_name;`|删除表中字段|
|`alter table table_name add field_name INT`|添加字段，并给出类型|
|`alter table table_name modify field_name CHAR(10);`|修改字段类型|
|`alter table table_name change field_old fileld_new BIgINT;`|同时修改字段名称类型|
|`alter table table_name rename to new_name;`|修改表名|
# 3. 事务
一系列数据库语句构成了事务。  
```msyql
begin; # 开启一个事务
***
***
savapoit savepoint-name; #申明一个保存点
rollback to savapoint_name # 回滚到保存点
rollback; # 事务回滚
***
***
commit;
```
# 4. 索引
创建索引时需要确保该索引是应用在SQL查询语句的条件（一般为where子句的条件）。
insert, update, delete表时不要使用索引。
## 创建索引
### 普通索引
- 修改表创建：`alter table table_name  add INDEX indexName(columnName);`
- 创建表的时候直接指定
```msyql
CREATE TABLE mytable(  
ID INT NOT NULL,   
username VARCHAR(16) NOT NULL,  
INDEX [indexName] (username(length))  
);  
```
### 唯一索引（索引列的值必须唯一，但允许有空值。）

有四种方式来添加数据表的索引：
```msyql
ALTER TABLE tbl_name ADD PRIMARY KEY (column_list): 该语句添加一个主键，这意味着索引值必须是唯一的，且不能为NULL。
ALTER TABLE tbl_name ADD UNIQUE index_name (column_list): 这条语句创建索引的值必须是唯一的（除了NULL外，NULL可能会出现多次）。
ALTER TABLE tbl_name ADD INDEX index_name (column_list): 添加普通索引，索引值可出现多次。
ALTER TABLE tbl_name ADD FULLTEXT index_name (column_list):该语句指定了索引为 FULLTEXT ，用于全文索引。
```
#### 删除索引

# 5. 临时表
```msyql
CREATE TEMPORARY TABLE 临时表名 AS
(
    SELECT *  FROM 旧的表名
    LIMIT 0,10000
);
```
# 6. 数据类型
## String类型
CHAR和VARCHAR类型相似，但在存储和检索它们的方式上有所不同。它们在最大长度和是否保留尾空格方面也有所不同。[区别](https://dev.mysql.com/doc/refman/8.0/en/char.html)
- CHAR：长度值0-255, 存储时，数据长度不足定义的长度时，从右边用空格补齐，查询的时候尾部的空格会被移除。适用于存储固定长度的数据，如手机号。
- VARCHAR：长度值0-65535，存储时，会在数据前加1字节或2字节的长度前缀，不会有空格补齐，存储数据实际的长度加上长度前缀。使用与不确定长度的数据，如人名，地名等。
# 一些问题
### 1. utf8和utf8mb4的区别?
mb4就是most bytes 4的意思，专门用来兼容四字节的unicode, utf8mb4是utf8的超集。mysql支持的 utf8 编码最大字符长度为 3 字节，如果遇到 4 字节的宽字符就会插入异常了。
### 2. 什么是Online DDL?
MySQL各版本，对于DDL的处理方式是不同的，主要有三种：
- Copy Table算法
- Inplace 算法
- Online方式: online方式支持DDL时不仅可以读，还可以写。Online DDL提供了在表更改过程中并发执行DML的功能。
### 3. Mysql有哪些数据库引擎，有什么区别
- InnoDB: Mysql默认的数据库引擎; 支持事务; 支持外键; 是聚集索引; 不保存表的总行数;
- MyISAM:不支持事务; 不支持外键; 是非聚集索引; 保存有表的总行数

## 4. 什么是聚集索引？
### 聚集索引
这种索引使得表中每行数据的物理存储顺序和索引的逻辑顺序一致。一个表只能包含一个聚集索引。clustered index的叶子节点保存着数据页。

对于mysql，聚集索引通常是表的主键，若无主键则为表中第一个非空的唯一的数据列，还是没有就采用innodb存储引擎为每行数据内置的ROWID作为聚集索引。
#### 使用情景
返回大型结果集的查询; 
### 非聚集索引
引中索引的逻辑顺序与行数据的物理存储顺序不同。
#### 使用情景

## 5. mysql索引使用的是什么算法？ 
大部分的索引(PRIMARY KEY, UNIQUE, INDEX, and FULLTEXT)使用的是B-trees。例外：spatial data类型上的索引使用的是R-trees; Memory表也支持哈希索引；InnoDB的FULLTEXT索引使用的是inverted lists.

# 错误及解决方法
1. `groupby`
   ```mysql
   select Name, max(Salary), DepartmentId
   from Employee
   group by DepartmentId;
   ```
> ERROR 1055 (42000): Expression #1 of SELECT list is not in GROUP BY clause and contains nonaggregated column 'menagerie.Employee.Name' which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by

怎么通过优化sql语句解决
