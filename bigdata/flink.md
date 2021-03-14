# 1. 概念
## State
- 有些算子仅仅简单地在一个时间点记录单个event，然而有些算子记录着多个events。这些算子被称为有状态地(Stateful).
- Flink需要记录State来实现容错，通过使用[checkpoints](https://ci.apache.org/projects/flink/flink-docs-release-1.11//ci.apache.org/projects/flink/flink-docs-release-1.11/dev/stream/state/checkpointing.html) 和 [savepoints](https://ci.apache.org/projects/flink/flink-docs-release-1.11//ci.apache.org/projects/flink/flink-docs-release-1.11/ops/state/savepoints.html).   

## Checkpointing 和 Savepoint
当前checkpoint和savepoint在实现上并没有什么区别，只是在概念不同。

checkpoint的生命周期是由flink框架本身管理，是由flink创建，拥有和释放，不需要用户参与。作为一种恢复和周期性触发的方法，设计目标是:1)轻量;2)尽可能快地恢复。检查点通常在用户终止作业之后被删除(除非显式地配置为保留检查点)。

而Savepoint是由用户创建、拥有和删除的，用于手动备份和恢复。例如Flink版本的更新、更改job graph、更改并行度等。Savepoint在工作终止后继续存在。保存点的生成和恢复成本可能会更高一些，并且更多地关注可移植性和对前面提到的作业更改的支持。

[Chandy-Lamport algorithm](http://research.microsoft.com/en-us/um/people/lamport/pubs/chandy.pdf) 
## State Backends
写在Data Stream API中的程序，常有几种保存状态的方法：
- 在被触发之前，窗口收集或聚合元素
- 转化函数用key/value state接口存储value
- 转换函数实现`CheckpointedFunction`接口来让本地变量容错。

状态在内部是如何表示的，以及它在checkpoint上是如何保存的以及在哪里保存的，都取决于所选的状态后端。

Flink自带三种状态后端
- MemoryStateBackend 默认
- FsStateBackend
- RocksDBStateBackend

`MemoryStateBackend`将状态保存在Java堆上，Key/value状态和窗口操作使用哈希表保存值。
`FsStateBackend`将状态快照保存在配置的文件路径上
`RocksDBStateBackend`将数据存在`RocksDB`中，RockDB存储在TaskManager数据文件夹里

## 设置状态后端
```java
StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
env.setStateBackend(new FsStateBackend("hdfs://namenode:40010/flink/checkpoints"));
```
# Flink 架构
![flink架构](https://ci.apache.org/projects/flink/flink-docs-release-1.12/fig/processes.svg)

Flink运行时由两种类型进程构成：一个***JobManager***和一个或多个***TaskManager***
### JobManager
该进程包含三个部分：
- **ResourceManager**   
ResourceManager负责资源分配，管理**task slots**(资源调度地单位)。
- **Dispatcher**  
提供一个REST接口来提交Flink应用去执行，为每一个job开启新的**job master**
- **JobMaster**  
负责管理一个**JobGraph**地执行
### TaskManager
执行一个dataflow的任务，一个TaskManager有一个或多个**task slot**
## WaterMark
### 什么是watermark
watermark的本质是一种时间戳，它用来处理事件乱序的问题。数据流中的 Watermark 用于表示 timestamp 小于 Watermark 的数据，都已经到达了，window 的执行也是由 Watermark 触发的。Watermark 可以理解成一个延迟触发机制，我们可以设置 Watermark 的延时时 长 t， 每 次 系 统 会 校 验 已 经 到 达 的 数 据 中 最 大 的 maxEventTime， 然 后 认 定eventTime 小于 maxEventTime - t 的所有数据都已经到达，如果有窗口的停止时间
等于 maxEventTime – t，那么这个窗口被触发执行。当 Flink 接收到每一条数据时，都会产生一条 Watermark，这条 Watermark就等于当前所有到达数据中的 maxEventTime - 延迟时长。
### 分布式处理时watermark的传递机制
向下游传递时采用广播传递。接收上游数据时，为每个上游分别设置一个watermark分区，再按照分区中值最小的watermark为准。
## State and Fault Tolerance in Batch Programs
在批处理程序中不使用*checkpointing*
# 2. DataStream
## 1.1什么是DataStream
DataStream是一组不可变的，可重复的数据集合。
## 2.2 Flink编程模式
Flink 程序包括以下几个步骤：
* 1 获取 `execution environment`
* 2 加载或初始化数据
* 3 使用算子对数据进行操纵
* 4 指出计算结果输出的地方
* 5 启动程序的执行

## 算子
### DataStrem算子
1) `map()` 1->1 
2) `flatMap()` 1->N
3) `filter()`
4) `keyBy()`
5) `reduce()` N->1
6) `Aggregations` 具体指`sum`, `min`, `max`, `minBy`, `maxBy`
7) `window()`
8) `windowAll()`
9) `apply()` in Window
10) `reduce()` in Window
11) `Aggregations` in Window 
12) `union()`
13) `join()` in Window
14) `intervalJoin()`
15) `coGroup()` in Window
16) `connect()`
17) `map()`, `flatmap()` On ConnectedStreams 
18) `Iterate()`
19) `project()`

### 物理分区
1) `partitionCustom()` 自定义分区
2) `shuffle()` 随机分区
3) `rebalance()` ？
4) `rescale()`
5) `broadcast()` 将元素广播大到每个分区

## 任务链
1) `startNewChain()`
2) `disableChaining()`
3) `slotSharingGroup()`
## 2.5 Window
### 窗口类型
- Time Window
	- Tumbling Window
	固定长度，不重叠
	- Sliding Window
	固定长度，可重叠
	- Session Window
	由一系列事件 + TimeGap组合而成
window()方法必须在KeyBy之后才能用

## 2.6 Watermark
Flink中处理乱序数据提供了三层保证
- watermark
- 允许延迟
- SideOutputLateness
## State    
Flink里的State可分为三种：
- Keyed state
根据数据的key来维护和访问
- Operator State
作用范围限定为算子任务
-  Broadcast State
# 端到端一致性
## 背压(Back Pressure)
背压指的是数据产生的速度快于下游处理所能消费的速度。

背压监测的工作原理是反复取回运行任务的压力样本。JobManager会不断地为作业任务触发`Task.isBackPressured()`.

背压是根据输出Buffer的可用性来判断的。如果没有可用的缓冲区(至少一个)用于输出，则表明任务存在背压。

### 怎么解决背压？
