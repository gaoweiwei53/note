# Hive 简介
## 1. 什么是hive
Hive本质：将HSQL转化为Mapreduce程序
(1) Hive处理的数据存储在HDFS
(2) Hive分析数据的底层实现是Mapreduce
(3) 运行在yarn上
## 2. Hive的结构
1) Client
2) Metastore  
推荐使用MySql
3）HDFS
4) Driver
- SQL sparser
- Query Optimizer
- Execution
## 3. 与数据库的对比
1) SQL语言相似
2) 不善于更新数据，不建议更新操作
3) 没有索引，查询的时候需要扫描整张表，加上MapReduce的特性，执行延迟高
4) 支持大数据规模
