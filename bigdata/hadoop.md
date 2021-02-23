# 1. 常见问题
1) 指定`hadoop-env.sh`里的`JAVA_HOME`路径，默认的设置可能不行
# 2. HDFS, Hive, Hbase的区别
- **HDFS**(Hadoop Distributed File System)是被设计用来运行在廉价机器上的分布式文件系统
- **Hive**是一个数据仓库工具，用来进行数据提取、转化、加载，它将结构化的数据文件映射为一张数据库表，并提供SQL查询功能，能将SQL语句转变成MapReduce任务来执行。
- **Hbase**是一个基于HDFS的分布式，面向列的非关系数据库
## WebUI
[HDFS](localhost:50070)

[Cluster](localhost:8088)
# Hadoop 3.x 新特性
## 1. jdk要求
由以前的jdk.7最低要求改为jdk1.8
## HDFS
1) 存储：新增纠删码存储技术
什么是纠删码？增加了校验码，通过增加一个校验码可得出任意个数据，类似方程与未知数的个数。
代价：用时间换空间，增加了技术的复杂性
3.0之前的hadoop采用备份的方法实现高可用
4) 支持2个以上的namenode
5) 文件系统：增加对微软Azure Data Lake和阿里云OSS文件系统的支持
6) 功能：新增datanode内部磁盘负载均衡功能，调用命令

## Yarn
1) YARN timeline service v.2: 提升了扩展性和可靠性
2) 支持Opportunistic Containers和Distributed Scheduling
3) 容器资源类型：支持对CPU和内存之外的资源配置，如GPU, 本地存储资源等
4) 新的图形化监控界面
## 其他
1) Client的jar引用优化：将hadoop client的第三方依赖以shading dependency的方式隔离在一个单一Jar包中，以避免hadoop依赖渗透到应用程序的类路径中
2) MapReduce任务本地优化：shuffle密集型的任务，性能提升30%以上
3) 优化了对守护进程和mr任务的堆管理配置
4) 第三方依赖包升级：jersey，netty，cglib等
