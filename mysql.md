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
```
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
```
CREATE TABLE mytable(  
ID INT NOT NULL,   
username VARCHAR(16) NOT NULL,  
INDEX [indexName] (username(length))  
);  
```
### 唯一索引（索引列的值必须唯一，但允许有空值。）

有四种方式来添加数据表的索引：
```
ALTER TABLE tbl_name ADD PRIMARY KEY (column_list): 该语句添加一个主键，这意味着索引值必须是唯一的，且不能为NULL。
ALTER TABLE tbl_name ADD UNIQUE index_name (column_list): 这条语句创建索引的值必须是唯一的（除了NULL外，NULL可能会出现多次）。
ALTER TABLE tbl_name ADD INDEX index_name (column_list): 添加普通索引，索引值可出现多次。
ALTER TABLE tbl_name ADD FULLTEXT index_name (column_list):该语句指定了索引为 FULLTEXT ，用于全文索引。
```
#### 删除索引

# 5. 临时表
```
CREATE TEMPORARY TABLE 临时表名 AS
(
    SELECT *  FROM 旧的表名
    LIMIT 0,10000
);
```

