# 1. SparkCore
## 概念
1) RDD: 提出这个概念的动机是之前的大数据框架是将计算得到的中间结果存在外存中，这样对那些需要迭代计算的任务很不适合，因为不同地计算需要用到中间结果，因此提出RDD这个抽象以能够不同地应用重用数据。
它本质是一种不可变地，分区地记录集合。RDD一个核心特性是每个RDD都记录了血缘，当某个RDD失败之后，它可以通过血缘来恢复。
3. 窄依赖指的是每一个父RDD的 Partition 最多被子RDD的一个 Partition 使用
4. 宽依赖指的是多个子 RDD的 Partition 会依赖同一个父RDD的 Partition，会引起 shuffle
5. Spark 三大数据结构
6) RDD
7) 广播变量
8) 累加器
RDD: 分布式数据集
广播变量：分布式只读共享变量
累加器：分布式只写共享变量
## 任务划分
RDD任务切分中间分为：Application、Job、Stage 和 Task
1) Application：初始化一个 SparkContext 即生成一个Application
2) Job：一个Action 算子就会生成一个 Job
3) Stage：根据RDD之间的依赖关系的不同将 Job 划分成不同的 Stage，遇到一个宽依赖 则划分一个 Stage。
4) Task：Stage 是一个 TaskSet，将 Stage 划分的结果发送到不同的 Executor 执行即为一个 Task。
##  RDD缓存
RDD通过 persist 方法或 cache 方法可以将前面的计算结果缓存，默认情况下 persist()会把数据以序列化的形式缓存在 JVM 的堆空间中。 但是并不是这两个方法被调用时立即缓存，而是触发后面的 action 时，该RDD将会 被缓存在计算节点的内存中，并供后面重用。
## RDD CheckPoint
Spark 中对于数据的保存除了持久化操作之外，还提供了一种检查点的机制，检查点（本质是通过将RDD 写入Disk 做检查点）是为了通过 lineage 做容错的辅助，lineage 过长 会造成容错成本过高，这样就不如在中间阶段做检查点容错，如果之后有节点出现问题而 丢失分区，从做检查点的RDD开始重做 Lineage，就会减少开销。检查点通过将数据写入 到HDFS 文件系统实现了RDD的检查点功能。 为当前RDD设置检查点。该函数将会创建一个二进制的文件，并存储到 checkpoint 目录中，该目录是用 SparkContext.setCheckpointDir()设置的。在 checkpoint 的过程中，该 RDD的所有依赖于父RDD中的信息将全部被移除。对RDD进行 checkpoint 操作并不会马 上被执行，必须执行Action 操作才能触发。
```scala
sc.setCheckpointDir()
rdd.checkpoint
```
RDD缓存和CheckPoint的区别是一个保存在内存中，后者保存在磁盘中
## 键值对RDD数据分区
Spark 目前支持**Hash 分区**和**Range 分区**，用户也可以自定义分区，Hash 分区为当前的默认分区，Spark 中分区器直接决定了RDD中分区的个数、RDD中每条数据经过 Shuffle 过程属于哪个分区和 Reduce 的个数。

注意：
- 只有Key-Value 类型的RDD才有分区的，非Key-Value 类型的RDD分区的值是None
- 每个RDD的分区 ID 范围：0~numPartitions-1，决定这个值是属于那个分区的。
 ### Hash 分区
 HashPartitioner 分区的原理：对于给定的 key，计算其 hashCode，并除以分区的个数取余，如果余数小于 0，则用余数+分区的个数（否则加 0），最后返回的值就是这个 key 所 属的分区 ID。
 ### Ranger 分区
 HashPartitioner 分区弊端：可能导致每个分区中数据量的不均匀，极端情况下会导致某些分区拥有RDD的全部数据。
 RangePartitioner 作用：将一定范围内的数映射到某一个分区内，尽量保证每个分区中数据量的均匀，而且分区与分区之间是有序的，一个分区中的元素肯定都是比另一个分 区内的元素小或者大，但是分区内的元素是不能保证顺序的。简单的说就是将一定范围内 的数映射到某一个分区内。实现过程为：
 第一步：先重整个RDD中抽取出样本数据，将样本数据排序，计算出每个分区的最大 key 值，形成一个Array[KEY]类型的数组变量 rangeBounds；
 第二步：判断 key 在 rangeBounds 中所处的范围，给出该 key 值在下一个RDD中的 分区 id 下标；该分区器要求 RDD中的KEY类型必须是可以排序的
 ### 自定义分区
 要实现自定义的分区器，你需要继承 org.apache.spark.Partitioner 类并实现下面三个方法。 
1) numPartitions: Int:返回创建出来的分区数。 
2) getPartition(key: Any): Int:返回给定键的分区编号(0 到numPartitions-1)。 
3) equals():Java 判断相等性的标准方法。这个方法的实现非常重要，Spark 需要用这个 方法来检查你的分区器对象是否和其他分区器实例相同，这样 Spark 才可以判断两个 RDD 的分区方式是否相同。
 ## 数据读取与保存
 Spark 的数据读取及数据保存可以从两个维度来作区分：文件格式以及文件系统。 文件格式分为：Text 文件、Json 文件、Csv 文件、Sequence 文件以及 Object 文件； 文件系统分为：本地文件系统、HDFS、HBASE 以及数据库。
 ## 累加器
 累加器用来对信息进行聚合，通常在向 Spark 传递函数时，比如使用 map() 函数或者用 filter() 传条件时，可以使用驱动器程序中定义的变量，但是集群中运行的每个任务都会 得到这些变量的一份新的副本，更新这些副本的值也不会影响驱动器中的对应变量。如果我 们想实现所有分片处理时更新共享变量的功能，那么累加器可以实现我们想要的效果。
 ### 自定义累加器
 实现自定义类型累加器需 要继承AccumulatorV2 并至少覆写下例中出现的方法。
 ## 广播变量（调优策略）
 广播变量用来高效分发较大的对象。向所有工作节点发送一个较大的只读值，以供一个或多个 Spark 操作使用。比如，如果你的应用需要向所有节点发送一个较大的只读查询 表，甚至是机器学习算法中的一个很大的特征向量，广播变量用起来都很顺手。 在多个并行操作中使用同一个变量，但是 Spark 会为每个任务分别发送。
```
scala> val broadcastVar = sc.broadcast(Array(1, 2, 3))
 broadcastVar: org.apache.spark.broadcast.Broadcast[Array[Int]] = Broadcast(35)
scala> broadcastVar.value res33: Array[Int] = Array(1, 2, 3)
```
使用广播变量的过程如下：
1) 通过对一个类型 T的对象调用 SparkContext.broadcast 创建出一个Broadcast[T]对
象。任何可序列化的类型都可以这么实现。 
2) 通过 value 属性访问该对象的值(在 Java 中为 value()方法)。 (3) 变量只会被发到各个节点一次，应作为只读值处理(修改这个值不会影响到别的节
点)。
# 2. Spark SQL
# 3. SparkStreaming
## 3.1 SparkStreaming提供两种类型的流数据源
* Basic Sources
* Advanced Sources

### 3.1.1 Spark Streaming Kafka
* **Receiver based Approach**
* **Direct Approach**

**Receiver based Approach**
基于receiver的方式是使用kafka消费者高阶API实现的。
对于所有的receiver，它通过kafka接收的数据会被存储于spark的executors上，底层是写入BlockManager中，默认200ms生成一个block（通过配置参数spark.streaming.blockInterval决定）。然后由spark streaming提交的job构建BlockRdd，最终以spark core任务的形式运行。
**Direct Approach**
它不是启动一个Receivers来连续不断地从Kafka中接收数据并写入到WAL中，而且简单地给出每个batch区间需要读取的偏移量位置，最后，每个batch的Job被运行，那些对应偏移量的数据在Kafka中已经准备好了。这些偏移量信息也被可靠地存储（checkpoint），在从失败中恢复可以直接读取这些偏移量信息。
[1](https://mp.weixin.qq.com/s?src=11&timestamp=1603546636&ver=2664&signature=ACVKfrVXmUC9A1ITkJx03XHhDwdACDQnII0AcBfUAImFf4OcRUxVsLWgZ3K0OeUPSwAID9I6ugfQwiwW0367K9*Crqdl5*ZZViIThwa5etR6VCVsRAXRFtOEAhebLwil&new=1)  [2](https://mp.weixin.qq.com/s?src=3&timestamp=1603546636&ver=1&signature=ShJYbp57pHD-Hat9W5PKXSzFrf*j0BAHONM0dDu2XVBUCEO23en73Lt7TSeaZRDrZrSeZUATwUBSoE3cbElQ2IP0Ybi-NlZv6vbI7Wq9i5mrt8AzlJUH6HRuxVkBmf8GTLeFT9khs9fFdeTzthBA3g==)
## 	Transformations on DStreams
Transformations允许改变来自输入DStream的数据, 分为无状态和有状态。无状态的transformation只能作用于每个采集区间中，无法保存每个batch的状态。而有状态的transformation可以保存之前的batch的计算结果，也就是状态，然后可以使用每个batch间的结果进行进一步计算。
#### UpdateStateByKey Operation
UpdateStateByKey Operation 算子允许在伴随着新的信息不断更新时，维护任意的状态(State)。通过两步来实现：
1. 定义状态
2. 定义状态更新函数：明确怎样使用之前和当前的数据更新状态。

Note: 使用 updateStateByKey 需要对检查点目录进行配置，会使用检查点来保存状态
## Transform
和Map()函数的逻辑类似，但是map函数是作用于数据上的，而transform作用于每个RDD上，可实现RDD-RDD的转换。
