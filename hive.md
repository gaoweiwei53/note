# 1. Hive 简介
## 1.1 什么是hive
Hive本质：将HSQL转化为Mapreduce程序  
(1) Hive处理的数据存储在HDFS  
(2) Hive分析数据的底层实现是Mapreduce  
(3) 运行在yarn上  
## 1.2 Hive的结构
- Client
- Metastore  
推荐使用MySql
- HDFS
- Driver
  - SQL sparser
  - Query Optimizer
  - Execution
## 1.3 与数据库的对比
- SQL语言相似
- 不善于更新数据，不建议更新操作
- 没有索引，查询的时候需要扫描整张表，加上MapReduce的特性，执行延迟高
- 支持大数据规模

## 1.4 Configure mysql metastore
1) install mysql and create a new user
```sql
create user 'hive_user'@'localhost' identified by '123';
grant all on metastore.* to 'hive_user'@'localhost';
```
if remote, replace 'localhost' with '%' or a ip

2) create `hive-site.xml` in `conf` folder.
```xml
<?xml version="1.0"?>    
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>    
<configuration>    
<property>        
    <name>javax.jdo.option.ConnectionURL</name>        
    <value>jdbc:mysql://localhost/metastore?createDatabaseIfNotExist=true</value>        
  </property>           
  <property>        
    <name>javax.jdo.option.ConnectionDriverName</name>        
    <value>com.mysql.cj.jdbc.Driver</value>        
  </property>           
  <property>        
    <name>javax.jdo.option.ConnectionUserName</name>        
    <value>hive_user</value>        
  </property>           
  <property>        
    <name>javax.jdo.option.ConnectionPassword</name>        
    <value>123</value>        
  </property>           
</configuration>
```
3) mv a [mysql-connector.jar](https://mvnrepository.com/artifact/mysql/mysql-connector-java) to `hive/lib` folder
4) 修改hadoop的`core-site.xml`文件
```xml
   <property>
        <name>hadoop.proxyuser.xxx.hosts</name>
        <value>*</value>
    </property>
    <property>
        <name>hadoop.proxyuser.xxx.groups</name>
        <value>*</value>
    </property>
```
其中xxx为beeline登录的用户名  

5) initial mysql metastore 
```bash
$HIVE_HOME/bin/schematool -dbType mysql -initSchema
```
6)  start metastore service  
```
$HIVE_HOME/bin/hive --service metastore
```  
7) start hiveserver2
```bash
$HIVE_HOME/bin/hiveserver2
$HIVE_HOME/bin/beeline -u jdbc:hive2://$HS2_HOST:$HS2_PORT
$HIVE_HOME/bin/beeline -u jdbc:hive2://localhost:10000
```
By default, it will be (localhost:10000), so the address will look like jdbc:hive2://localhost:10000.  
A Web User Interface (UI) for HiveServer2 provides configuration, logging, metrics and active session information. The Web UI is available at port 10002 (127.0.0.1:10002) by default. 
# 2. 语句
`show create table test;`查看建表语句

`where`后不能用分组函数，`having` 在 `group by`语句后生效，只用于`group by`分组统计语句。
Order By：全局排序，只有一个 Reducer。

Sort By：对于大规模的数据集 order by 的效率非常低。在很多情况下，并不需要全局排
序，此时可以使用 sort by。

Sort by 为每个 reducer 产生一个排序文件。每个 Reducer 内部进行排序，对全局结果集来说不是排序。

# 3. 外部表、内部表(管理表)
## 内部表(Managed Table)与外部表的区别
在hive中删除管理表的时候，元数据和HDFS中的文件都会被一起删除，而删除外部表的时候，只会删除相应的元数据，hdfs里的文件不会被删除。
## 使用场景
一般将收集到的原始数据，如日志，存为**外部表**，在外部表的基础上进行分析，产生的中间表和结果表使用**管理表**进行存储。
