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
### 首次读流程
1) Client从ZooKeeper中读取hbase:meta表
2) Client从hbase:meta中获取想要操作的region的位置信息，并且将hbase:meta缓存在Client端，用于后续的操作
3) 当一个RegionServer宕机而执行重定位之后，Client需要重新获取新的hase:meta信息进行缓存
### 其他时候读
找到要读取数据的region所在的RegionServer，然后按照以下顺序进行读取：先去BlockCache读取，若BlockCache没有，则到Memstore读取，若MemStore中没有，则到HFile中读取。
## 4.2 写流程
找到要写入数据的region所在的RegionServer，然后将数据先写到WAL中，然后再将数据写到MemStore等待刷新，回复客户端写入完成。
##  HRegionServer宕机如何处理？
1）ZooKeeper会监控HRegionServer的上下线情况，当ZK发现某个HRegionServer宕机之后会通知HMaster进行失效备援；
2）该HRegionServer会停止对外提供服务，就是它所负责的region暂时停止对外提供服务
3）HMaster会将该HRegionServer所负责的region转移到其他HRegionServer上，并且会对HRegionServer上存在memstore中还未持久化到磁盘中的数据进行恢复
4）这个恢复的工作是由WAL重播来完成，这个过程如下：

wal实际上就是一个文件，存在/hbase/WAL/对应RegionServer路径下

宕机发生时，读取该RegionServer所对应的路径下的wal文件，然后根据不同的region切分成不同的临时文件recover.edits

当region被分配到新的RegionServer中，RegionServer读取region时会进行是否存在recover.edits，如果有则进行恢复
