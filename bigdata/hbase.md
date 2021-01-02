# 1. 什么是Hbase
# 2. 数据模型
- Namespace: 相当于数据库的概念，里面包含一组表。
- Column Family:
- Row
- Column
- Version: 时间戳
- Region：由每个Column Family的Store组成。
- Store：保存着一个Memtore或多个HFiles，
- Memstore：保存着内存中的Store的变化
- Hfile/Store File: 磁盘中的数据
- WAl( Write Ahead Log): WAL记录着Hbase中所有的数据的变化
如果在Menmstore flush之前RegionServer挂了，WAL可以保证数据的改变可以重新实施。
- Flush: Memstore到Hfile的过程，将数据从内存中写到磁盘上。
- Master:负责监控所有的RegionServer，是元数据改变的接口。
- RegionServer：负责管理Region

*{row, column, vesion}* 唯一确定一个**cell**
# 3. 安装配置
问题：启动之后Hmaster不久就消失了  
解决：在hbase-site.xml中添加
```xml
  <property>    
    <name>hbase.unsafe.stream.capability.enforce</name>    
    <value>false</value>    
  </property> 
```
# 4. 读写流程
## 4.1 读流程
1) Client从ZooKeeper中读取hbase:meta表
2) Client从hbase:meta中获取想要操作的region的位置信息，并且将hbase:meta缓存在Client端，用于后续的操作
3) 当一个RegionServer宕机而执行重定位之后，Client需要重新获取新的hase:meta信息进行缓存
## 4.2 写流程
