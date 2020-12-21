# 1. 常见问题
1) 指定`hadoop-env.sh`里的`JAVA_HOME`路径，默认的设置可能不行
# 2. HDFS, HIVE, HBSE的区别
- **HDFS**(Hadoop Distributed File System)是被设计用来运行在廉价机器上的分布式文件系统
- **Hive**是一个数据仓库工具，用来进行数据提取、转化、加载，它将结构化的数据文件映射为一张数据库表，并提供SQL查询功能，能将SQL语句转变成MapReduce任务来执行。
- **Hbase**是一个基于HDFS的分布式，面向列的非关系数据库
