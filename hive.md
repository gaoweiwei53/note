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
