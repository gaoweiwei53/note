# 1. 常见问题
1) 指定`hadoop-env.sh`里的`JAVA_HOME`路径，默认的设置可能不行
## HDFS, Hive, Hbase的区别
- **HDFS**(Hadoop Distributed File System)是被设计用来运行在廉价机器上的分布式文件系统
- **Hive**是一个数据仓库工具，用来进行数据提取、转化、加载，它将结构化的数据文件映射为一张数据库表，并提供SQL查询功能，能将SQL语句转变成MapReduce任务来执行。
- **Hbase**是一个基于HDFS的分布式，面向列的非关系数据库
## WebUI
[HDFS](localhost:50070)

[Cluster](localhost:8088)
# 2. 新版本特性
## Hadoop 3.x 新特性
### jdk要求
由以前的jdk.7最低要求改为jdk1.8
### HDFS
1) 存储：新增纠删码存储技术
什么是纠删码？增加了校验码，通过增加一个校验码可得出任意个数据，类似方程与未知数的个数。
代价：用时间换空间，增加了技术的复杂性
3.0之前的hadoop采用备份的方法实现高可用
4) 支持2个以上的namenode
5) 文件系统：增加对微软Azure Data Lake和阿里云OSS文件系统的支持
6) 功能：新增datanode内部磁盘负载均衡功能，调用命令
### Yarn
1) YARN timeline service v.2: 提升了扩展性和可靠性
2) 支持Opportunistic Containers和Distributed Scheduling
3) 容器资源类型：支持对CPU和内存之外的资源配置，如GPU, 本地存储资源等
4) 新的图形化监控界面
### 其他
1) Client的jar引用优化：将hadoop client的第三方依赖以shading dependency的方式隔离在一个单一Jar包中，以避免hadoop依赖渗透到应用程序的类路径中
2) MapReduce任务本地优化：shuffle密集型的任务，性能提升30%以上
3) 优化了对守护进程和mr任务的堆管理配置
4) 第三方依赖包升级：jersey，netty，cglib等
## Hadoop 3.30新特性
1) 第一个支持ARM架构的版本
2) 完全支持 Java 11
3) 实现兼容Tencent Cloud COS文件系统
4) HDFS支持在SCM(storage class memory)读取缓存，但是还没有用到SCM持久化的功能
# 3. MapReduce
## 3.1 概述
MapReduce job 通常将输入的数据集分割成独立的数据块，这些数据块由map任务以完全并行的方式处理。框架**对map的输出进行排序**，然后将其输入到reduce任务。通常，作业的输入和输出都存储在文件系统中。

MapReduce框架由一个**master**  *ResourceManager*, 每一个节点中的worker *NodeManager* 和每个aplication的*MRAppMaster*组成

Mapper计算的结果首先1) 输出到环形缓冲区(默认100M)，环形缓冲区分为两部分，一部分存数据，一部分存数据的索引。同时标记数据的分区，并对分区内部的数据进行快速排序(对索引进行排序)。当写入的数据达到80%后会溢写到磁盘。2) 溢写之前会产生多个溢写文件，会对这些溢写文件按标记的分区分别进行归并排序，这样就可以保证每个分区内部有序。4) Combiner(可选): 有时会在每个分区内部的数据进行预聚合(如wordcount例子中，将相同key的数据聚合)。5) 压缩数据 6) 写入磁盘 7)Reducer按标记的分区分别读取Mapper的数据结果，读取到内存中，若内存不够则再次写入到磁盘中 8) 对每个map来的数据进行归并排序(一个分区的数据来自于多个map)。9) 按照相同的key分组 10) 执行reduce方法。
## 3.2 Inputs and Outputs
MapReduce框架操作的是键值<key,value>, key 和 value类需要被框架序列化，因此他们需要实现`Writable`接口. 此外, `key`类需要实现 `WritableComparable`接口以能够完成框架对其**排序**

Input and Output types of a MapReduce job:
```
(input) <k1, v1> -> map -> <k2, v2> -> combine -> <k2, v2> -> reduce -> <k3, v3> (output)
```
## 3.3 Mapper
Mapper是将输入数据转换为中间记录的单个任务。转换后的中间数据不需要与输入数据的类型相同。

Hadoop框架为每个由`InputFormat`产生的`InputSplit`, 生成一个map任务。

总的来说, `mapper` 的实现通过 `Job.setMapperClass(Class)` 方法被传递给`job`. 框架然后为每个在`InputSplit`里的key/value对调用 `map(WritableComparable, Writable, Context)`. 应用然后可以重写`cleanup(Context)`方法来执行任何需要的清理。

输出对不需要和输入对的类型一样.一个给定的输入对可能被map到0到很多个输出对。输出对通过调用的`context.write(WritableComparable, Writable)`方法被收集。

应用可以使用`Counter`来报告统计信息。

所有的给定的key对应的中间value接着会被框架分组, 传给`Reducer`，最后输出。用户可以通过`Job.setGroupingComparatorClass(Class)`制定一个` Comparator`控制这个分组过程.

`Mapper`的输出会被排序然后被分到每个`Reducer`对应的分区上. 总的分区的数量和这个job的`Reduce`任务数量相同。用户可以通过实现自定义的`Partitioner`来控制哪个keys (and hence records)去往哪个

用户可以选择性的通过`Job.setCombinerClass`指定一个`combiner`, 来在本地聚合中间输出，者可以减小`Mapper`到`Reducer`的之间的传输量。

中间的已排序的输出总是被存储为`(key-len, key, value-len, value)`的格式. 应用可以通过配置文件配置`CompressionCodec`, 控制中间输出，是否或怎样被压缩。.

### 多少个Maps合适?
`map`的数量通常由输入数据的大小决定，即输入数据总的Bloks数量

合适的数量是每个节点大约10-100个，之前对cpu-light的map任务，已经被设置300个.

因此，你希望10TB的输入数据的每个block大小为128M, 那么会有82,000 个maps, 除非通过`Configuration.set(MRJobConfig.NUM_MAPS, int)`来设置更多。
## 2.4 Reducer
Reducer的作用是将一系列中间输出的同一个key对应的values减少为更小规模的values集合

Reduce的数量，用户可通过`Job.setNumReduceTasks(int)`来设置.

总的来说，Reducer的实现通过`Job.setReducerClass(Class)`方法传给job的，也可以重写它来初始化它自己。框架然后为输出组里的每个`<key, (list of values)> `调用`reduce(WritableComparable, Iterable<Writable>, Context)`. 然后应用也可以重写`cleanup(Context)`方法来执行任何需要的清理。

> Reducer has 3 primary phases: **shuffle**, **sort** and **reduce**.

### Shuffle
输入到Reducer里的是**已排序的mappers输出**, 在这个阶段框架通过`HTTP`获取所有`mapper`输出的相关分区。
### Sort
这在阶段，框架通过`key`把reducer的输入分组(因为不同的mapper可能输出了相同的key)

*shuffe*和*sort*阶段是同时发生的。

### Secondary Sort
如果中间key分组的规则需要与`reduce`之前对key分组的规则不同，则可以通过`Job.setSortComparatorClass(Class)`指定一个` Comparator`。因为可以使用`Job.setGroupingComparatorClass(Class)`来控制中间key的分组方式。这些可以一起使用来模拟对value的第二次排序。

### Reduce
在这个阶段，`reduce(WritableComparable, Iterable<Writable>, Context)`会被为，已分组的输入中的每一个` <key, (list of values)> `对而调用。

Reduce任务的输出常通过`Context.write(WritableComparable, Writable)`被写入文件系统。

应用也可以使用`Counter`来报告统计信息。

`Reducer`的输出是没有被排序的。？？

### 多少个Reduces合适?
合适的reduce数量应该是 `0.95~1.75 * (<no. of nodes> * <no. of maximum containers per node>)`.

如果是0.95，所有的reduce都可以立即启动，并在mapps完成时开始传输maps输出。若是1.75，速度更快的，在负载平衡方面做得更好节点，将完成第一轮reduce，并启动第二轮reduce.

增加reduce数量增加会增加框架开销，但会提高负载平衡并降低故障成本。

The scaling factors above are slightly less than whole numbers to reserve a few reduce slots in the framework for speculative-tasks and failed tasks.

### 没有Reducer
如果不需要reduce任务，则可以将reduce-tasks的数量设置为零。

在这种情况下，map任务的输出直接进入文件系统，进入`FileOutputFormat.setOutputPath(Job, Path)`设置的输出路径。在将map输出写入文件系统之前，框架不会对它们进行排序。

## 3.5 分区器 Partitioner
Partitioner partitions the key space.

Partitioner控制中间map输出的key的分区。通常通过哈希函数。分区总数与job的reduce任务数相同。

`HashPartitioner`是默认的分区器.

## 3.6 Counter
`Counter`是MapReduce应用程序报告统计数据的工具。

Mapper and Reducer可以使用 `Counter`来报告统计数据.

Hadoop MapReduce附带了一个通用的mappers、reducers和分区器库。
# 4. HDFS

# 5. Yarn
Yarn的思想是将资源管理和调度/监测分离。Yarn的做法是有一个全局的ResourceManager(AM)和每个应用的ApplicationMaster(AM).一个应用就是一个job.

ResourceManager和NodeManager组成了数据计算框架。ResourceManager负责所有应用程序之间的资源调度。NodeManager是每台机器的框架代理，负责监控Containers的资源使用情况(cpu、内存、磁盘、网络)，并将这些情况报告给ResourceManager/Scheduler。

ResourceManager有两个主要组件:
- Scheduler  
Scheduler负责将资源分配给各种正在运行的应用程序，这些应用程序受到的容量、队列等限制。Scheduler是纯粹的调度器，因为它不执行对应用程序状态的监视或跟踪。此外，它不能保证由于应用程序失败或硬件失败而重新启动失败的任务。Scheduler根据应用程序的资源需求执行调度功能;它是基于resource Container的抽象概念来实现的，resource Container包含了内存、cpu、磁盘、网络等元素。
- ApplicationsManager  
ApplicationsManager负责接受job提交请求，调度Contaniner以启动ApplicationMaster，并负责ApplicationMaster失败重启。，每个应用程序的ApplicationMaster负责与Scheduler协商适当的资源容器，跟踪它们的状态并监控进程。


- CapacityScheduler
- FairScheduler



##NodeManager
负责启动和管理节点上的container。container执行AppMaster指定的任务。
