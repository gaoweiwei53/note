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
|操作表|
|`show tables;`||
|`create table tablename(***);`|建表|
|`describe/desc tablename;`|查看表内容|
|`load data local infile '/path/file.txt' into table pet;`|从文件中导入|
|`insert into pet values ();`|自己创建|
|`drop TABLE table_name;`|删除表|
|`update table_name set field1=new-value1,**`|修改表内数据|
|`delete from table_name [where **]`|删除数据|
## 3. 事务
一系列数据库语句构成了事务。  
```bash
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
