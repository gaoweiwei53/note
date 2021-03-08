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
# 2. MapReduce
## 概述
MapReduce job 通常将输入的数据集分割成独立的数据块，这些数据块由map任务以完全并行的方式处理。框架**对map的输出进行排序**，然后将其输入到reduce任务。通常，作业的输入和输出都存储在文件系统中。

MapReduce框架由一个**master**  *ResourceManager*, 每一个节点中的worker *NodeManager* 和每个aplication的*MRAppMaster*组成

## Inputs and Outputs
MapReduce框架操作的是键值<key,value>, key 和 value类需要被框架序列化，因此他们需要实现`Writable`接口. 此外, `key`类需要实现 `WritableComparable`接口以能够完成框架对其**排序**

Input and Output types of a MapReduce job:
```
(input) <k1, v1> -> map -> <k2, v2> -> combine -> <k2, v2> -> reduce -> <k3, v3> (output)
```
## Mapper
## Reduce
Mapper是将输入数据转换为中间记录的单个任务。转换后的中间数据不需要与输入数据的类型相同。

Hadoop框架为每个由`InputFormat`产生的`InputSplit`, 生成一个map任务。
Overall, mapper implementations are passed to the job via Job.setMapperClass(Class) method. The framework then calls map(WritableComparable, Writable, Context) for each key/value pair in the InputSplit for that task. Applications can then override the cleanup(Context) method to perform any required cleanup.

Output pairs do not need to be of the same types as input pairs. A given input pair may map to zero or many output pairs. Output pairs are collected with calls to context.write(WritableComparable, Writable).

Applications can use the Counter to report its statistics.

All intermediate values associated with a given output key are subsequently grouped by the framework, and passed to the Reducer(s) to determine the final output. Users can control the grouping by specifying a Comparator via Job.setGroupingComparatorClass(Class).

The Mapper outputs are sorted and then partitioned per Reducer. The total number of partitions is the same as the number of reduce tasks for the job. Users can control which keys (and hence records) go to which Reducer by implementing a custom Partitioner.

Users can optionally specify a combiner, via Job.setCombinerClass(Class), to perform local aggregation of the intermediate outputs, which helps to cut down the amount of data transferred from the Mapper to the Reducer.

The intermediate, sorted outputs are always stored in a simple (key-len, key, value-len, value) format. Applications can control if, and how, the intermediate outputs are to be compressed and the CompressionCodec to be used via the Configuration.
Overall, mapper implementations are passed to the job via Job.setMapperClass(Class) method. The framework then calls map(WritableComparable, Writable, Context) for each key/value pair in the InputSplit for that task. Applications can then override the cleanup(Context) method to perform any required cleanup.

Output pairs do not need to be of the same types as input pairs. A given input pair may map to zero or many output pairs. Output pairs are collected with calls to context.write(WritableComparable, Writable).

Applications can use the Counter to report its statistics.

All intermediate values associated with a given output key are subsequently grouped by the framework, and passed to the Reducer(s) to determine the final output. Users can control the grouping by specifying a Comparator via Job.setGroupingComparatorClass(Class).

The Mapper outputs are sorted and then partitioned per Reducer. The total number of partitions is the same as the number of reduce tasks for the job. Users can control which keys (and hence records) go to which Reducer by implementing a custom Partitioner.

Users can optionally specify a combiner, via Job.setCombinerClass(Class), to perform local aggregation of the intermediate outputs, which helps to cut down the amount of data transferred from the Mapper to the Reducer.

The intermediate, sorted outputs are always stored in a simple (key-len, key, value-len, value) format. Applications can control if, and how, the intermediate outputs are to be compressed and the CompressionCodec to be used via the Configuration.

How Many Maps?
The number of maps is usually driven by the total size of the inputs, that is, the total number of blocks of the input files.

The right level of parallelism for maps seems to be around 10-100 maps per-node, although it has been set up to 300 maps for very cpu-light map tasks. Task setup takes a while, so it is best if the maps take at least a minute to execute.

Thus, if you expect 10TB of input data and have a blocksize of 128MB, you’ll end up with 82,000 maps, unless Configuration.set(MRJobConfig.NUM_MAPS, int) (which only provides a hint to the framework) is used to set it even higher.

Reducer
Reducer reduces a set of intermediate values which share a key to a smaller set of values.

The number of reduces for the job is set by the user via Job.setNumReduceTasks(int).

Overall, Reducer implementations are passed the Job for the job via the Job.setReducerClass(Class) method and can override it to initialize themselves. The framework then calls reduce(WritableComparable, Iterable<Writable>, Context) method for each <key, (list of values)> pair in the grouped inputs. Applications can then override the cleanup(Context) method to perform any required cleanup.

Reducer has 3 primary phases: shuffle, sort and reduce.

Shuffle
Input to the Reducer is the sorted output of the mappers. In this phase the framework fetches the relevant partition of the output of all the mappers, via HTTP.

Sort
The framework groups Reducer inputs by keys (since different mappers may have output the same key) in this stage.

The shuffle and sort phases occur simultaneously; while map-outputs are being fetched they are merged.

Secondary Sort
If equivalence rules for grouping the intermediate keys are required to be different from those for grouping keys before reduction, then one may specify a Comparator via Job.setSortComparatorClass(Class). Since Job.setGroupingComparatorClass(Class) can be used to control how intermediate keys are grouped, these can be used in conjunction to simulate secondary sort on values.

Reduce
In this phase the reduce(WritableComparable, Iterable<Writable>, Context) method is called for each <key, (list of values)> pair in the grouped inputs.

The output of the reduce task is typically written to the FileSystem via Context.write(WritableComparable, Writable).

Applications can use the Counter to report its statistics.

The output of the Reducer is not sorted.

How Many Reduces?
The right number of reduces seems to be 0.95 or 1.75 multiplied by (<no. of nodes> * <no. of maximum containers per node>).

With 0.95 all of the reduces can launch immediately and start transferring map outputs as the maps finish. With 1.75 the faster nodes will finish their first round of reduces and launch a second wave of reduces doing a much better job of load balancing.

Increasing the number of reduces increases the framework overhead, but increases load balancing and lowers the cost of failures.

The scaling factors above are slightly less than whole numbers to reserve a few reduce slots in the framework for speculative-tasks and failed tasks.

Reducer NONE
It is legal to set the number of reduce-tasks to zero if no reduction is desired.

In this case the outputs of the map-tasks go directly to the FileSystem, into the output path set by FileOutputFormat.setOutputPath(Job, Path). The framework does not sort the map-outputs before writing them out to the FileSystem.

Partitioner
Partitioner partitions the key space.

Partitioner controls the partitioning of the keys of the intermediate map-outputs. The key (or a subset of the key) is used to derive the partition, typically by a hash function. The total number of partitions is the same as the number of reduce tasks for the job. Hence this controls which of the m reduce tasks the intermediate key (and hence the record) is sent to for reduction.

HashPartitioner is the default Partitioner.

Counter
Counter is a facility for MapReduce applications to report its statistics.

Mapper and Reducer implementations can use the Counter to report statistics.

Hadoop MapReduce comes bundled with a library of generally useful mappers, reducers, and partitioners.

