# Hive 简介
## 1. 什么是hive
Hive本质：将HSQL转化为Mapreduce程序  
(1) Hive处理的数据存储在HDFS  
(2) Hive分析数据的底层实现是Mapreduce  
(3) 运行在yarn上  
## 2. Hive的结构
- Client
- Metastore  
推荐使用MySql
- HDFS
- Driver
  - SQL sparser
  - Query Optimizer
  - Execution
## 3. 与数据库的对比
- SQL语言相似
- 不善于更新数据，不建议更新操作
- 没有索引，查询的时候需要扫描整张表，加上MapReduce的特性，执行延迟高
- 支持大数据规模

# Configure mysql metastore
1) install mysql and create a new user
```sql
create user 'hive_user'@'localhost' identified by '123';
grant all on metastore.* to 'hive_user'@'localhost';
```
if remote, replace 'localhost' with '%' or a ip
2) create `hive-site.xml` in `conf` folder
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
4) initial mysql metastore 
```bash
$HIVE_HOME/bin/schematool -dbType mysql -initSchema
```
