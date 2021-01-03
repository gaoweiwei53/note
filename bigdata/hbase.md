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
在shell中使用
|Command|function|
|-|-|
|`help`|查看相关命令|
|`help "command"`|查看命令用法|


# 4. 读写流程
读写过程不涉及Master，因为读写属于DML，而Master负责DDL，RegionServer负责DML.
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
1) ZooKeeper会监控HRegionServer的上下线情况，当ZK发现某个HRegionServer宕机之后会通知HMaster进行失效备援；
2) 该HRegionServer会停止对外提供服务，就是它所负责的region暂时停止对外提供服务
3) HMaster会将该HRegionServer所负责的region转移到其他HRegionServer上，并且会对HRegionServer上存在memstore中还未持久化到磁盘中的数据进行恢复
4) 这个恢复的工作是由WAL重播来完成，这个过程如下：

wal实际上就是一个文件，存在/hbase/WAL/对应RegionServer路径下

宕机发生时，读取该RegionServer所对应的路径下的wal文件，然后根据不同的region切分成不同的临时文件recover.edits

当region被分配到新的RegionServer中，RegionServer读取region时会进行是否存在recover.edits，如果有则进行恢复
## Flush过程
1) 当 MemStore 数据达到阈值（默认是 128M，老版本是 64M），将数据刷到硬盘，将内存中的数据删除，同时删除HLog 中的历史数据； 
2) 并将数据存储到HDFS 中； 
3) 在HLog 中做标记点
## 数据合并过程
1) 当数据块达到 4 块，Hmaster 触发合并操作，Region 将数据块加载到本地，进行合并； 
2) 当合并的数据超过 256M，进行拆分，将拆分后的 Region 分配给不同的 HregionServer 管理； 
3) 当HregionServer宕机后，将HregionServer上的hlog拆分，然后分配给不同的HregionServer 加载，修改.META. 
4) 注意：HLog 会同步到HDFS。
## StoreFile Compaction
由于metastore每次刷新都会生成一个HFile, 有时需要减少HFile的个数，以及清理掉过期和删除的数据，这个过程就是StoreFile Compaction.

Compaction分为两种：
- Minor Compaction: 会将一部分较小的，邻近的HFile合并，不会清理过期和删除的数据
- Major Compaction: 会将一个Store下所有Hfile合并成一个大的HFile，会清理过期和删除的数据
# 5. Hbase优化
## 高可用
在HBase 中Hmaster 负责监控RegionServer 的生命周期，均衡RegionServer 的负载，如果 Hmaster 挂掉了，那么整个 HBase 集群将陷入不健康的状态，并且此时的工作状态并不 会维持太久。所以HBase 支持对Hmaster 的高可用配置。
## 预分区
每一个 region 维护着 startRow 与 endRowKey，如果加入的数据符合某个 region 维护的rowKey 范围，则该数据交给这个 region 维护。那么依照这个原则，我们可以将数据所要投 放的分区提前大致的规划好，以提高HBase 性能。
## RowKey 设计
一条数据的唯一标识就是 rowkey，那么这条数据存储于哪个分区，取决于 rowkey 处于哪个一个预分区的区间内，设计rowkey的主要目的 ，就是让数据均匀的分布于所有的region 中，在一定程度上防止数据倾斜。
> 设计原则
- 散列性
- 唯一性
- 长度原则

> 方法
1) 生成随机数、hash、散列值
2) 字符串反转
3) 字符串拼接
## flush、compact、split 机制
当MemStore 达到阈值，将 Memstore 中的数据 Flush 进 Storefile；compact 机制则是把 flush 出来的小文件合并成大的 Storefile 文件。split 则是当 Region 达到阈值，会把过大的 Region 一分为二。

