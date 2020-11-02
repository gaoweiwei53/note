# 1. 概念
## State
- 有些算子仅仅简单地在一个时间点记录单个event，然而有些算子记录着多个events。这些算子被称为有状态地(Stateful).
- Flink需要记录State来实现容错，通过使用[checkpoints](https://ci.apache.org/projects/flink/flink-docs-release-1.11//ci.apache.org/projects/flink/flink-docs-release-1.11/dev/stream/state/checkpointing.html) 和 [savepoints](https://ci.apache.org/projects/flink/flink-docs-release-1.11//ci.apache.org/projects/flink/flink-docs-release-1.11/ops/state/savepoints.html).   

## Checkpointing
Flink容错机制的核心部分是画出分布式数据流和算子state的一致快照snapshots.
It is inspired by the standard [Chandy-Lamport algorithm](http://research.microsoft.com/en-us/um/people/lamport/pubs/chandy.pdf) for distributed snapshots and is specifically tailored to Flink’s execution model.
## State Backends
## Savepoints
## WaterMark
### 什么是watermark
watermark的本质是一种时间戳，它可用来处理乱序事件。数据流中的 Watermark 用于表示 timestamp 小于 Watermark 的数据，都已经到达了，window 的执行也是由 Watermark 触发的。Watermark 可以理解成一个延迟触发机制，我们可以设置 Watermark 的延时时 长 t， 每 次 系 统 会 校 验 已 经 到 达 的 数 据 中 最 大 的 maxEventTime， 然 后 认 定eventTime 小于 maxEventTime - t 的所有数据都已经到达，如果有窗口的停止时间
等于 maxEventTime – t，那么这个窗口被触发执行。当 Flink 接收到每一条数据时，都会产生一条 Watermark，这条 Watermark就等于当前所有到达数据中的 maxEventTime - 延迟时长，也就是说，Watermark是由数据携带的，一旦数据携带的 Watermark 比当前未触发的窗口的停止时间要晚，那么就会触发相应窗口的执行。由于 Watermark 是由数据携带的，因此，如果运行过程中无法获取新的数据，那么没有被触发的窗口将永远都不被触发。
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

The `StreamExecutionEnvironment` is the basis for all Flink programs. You can obtain one using these static methods on `StreamExecutionEnvironment`:
```java
getExecutionEnvironment()

createLocalEnvironment()

createRemoteEnvironment(String host, int port, String... jarFiles)
```
一般使用`getExecutionEnvironment() `就行了。在IDE里运行代码时，会在本机上创建运行环境，而在命令行中运行jar包时，会在集群上创建环境。
**指定数据源**：
```java
final StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();

DataStream<String> text = env.readTextFile("file:///path/to/file");
```
**使用算子**
```java
// This will create a new DataStream by converting every String in the original collection to an Integer.
DataStream<String> input = ...;

DataStream<Integer> parsed = input.map(new MapFunction<String, Integer>() {
    @Override
    public Integer map(String value) {
        return Integer.parseInt(value);
    }
});
```
创建Sink
```java
writeAsText(String path)

print()
```
最后需要触发程序的执行，通过调用`execute()`
The `execute()` method will wait for the job to finish and then return a `JobExecutionResult`, this contains execution times and accumulator results.

If you don’t want to wait for the job to finish, you can trigger asynchronous job execution by calling `executeAysnc()` on the `StreamExecutionEnvironment`. It will return a `JobClient` with which you can communicate with the job you just submitted. For instance, here is how to implement the semantics of `execute()` by using `executeAsync()`.
```java
final JobClient jobClient = env.executeAsync();

final JobExecutionResult jobExecutionResult = jobClient.getJobExecutionResult(userClassloader).get();
```	
## 2.3 DataSources
自定义Source
`StreamExecutionEnvironment.addSource(sourceFunction)` 
non-parallel sources, or by implementing the `ParallelSourceFunction` interface or extending the `RichParallelSourceFunction` for parallel sources.
* File-based:
* Socket-based:
* Collection-based:
* Custom: -   `addSource`  - Attach a new source function. For example, to read from Apache Kafka you can use  `addSource(new FlinkKafkaConsumer010<>(...))`. See  [connectors](https://ci.apache.org/projects/flink/flink-docs-release-1.11/dev/connectors/index.html)  for more details.

## 2.4 DataStream Transformations
Please see [operators](https://ci.apache.org/projects/flink/flink-docs-release-1.11/dev/stream/operators/index.html) for an overview of the available stream transformations.
## Data Sinks
Note that the `write*()` methods on `DataStream` are mainly intended for debugging purposes. They are not participating in Flink’s checkpointing, this means these functions usually have at-least-once semantics.

For reliable, exactly-once delivery of a stream into a file system, use the `flink-connector-filesystem`. Also, custom implementations through the `.addSink(...)` method can participate in Flink’s checkpointing for exactly-once semantics.
## Iterations
首先，定义一个**IterativeStream**
```java
IterativeStream<Integer> iteration = input.iterate();
```
接着指定循环中的操作
```java
DataStream<Integer> iterationBody = iteration.map(/* this is executed many times */);
```
o close an iteration and define the iteration tail, call the `closeWith(feedbackStream)` method of the `IterativeStream`. The DataStream given to the `closeWith` function will be fed back to the iteration head. A common pattern is to use a filter to separate the part of the stream that is fed back, and the part of the stream which is propagated forward. These filters can, e.g., define the “termination” logic, where an element is allowed to propagate downstream rather than being fed back.
```java
iteration.closeWith(iterationBody.filter(/* one part of the stream */));
DataStream<Integer> output = iterationBody.filter(/* some other part of the stream */);
```
## Execution Parameters
The  `StreamExecutionEnvironment`  contains the  `ExecutionConfig`  which allows to set job specific configuration values for the runtime.

Please refer to  [execution configuration](https://ci.apache.org/projects/flink/flink-docs-release-1.11/dev/execution_configuration.html)  for an explanation of most parameters. These parameters pertain specifically to the DataStream API:

-   `setAutoWatermarkInterval(long milliseconds)`: Set the interval for automatic watermark emission. You can get the current value with  `long getAutoWatermarkInterval()`

 ### Fault Tolerance[](https://ci.apache.org/projects/flink/flink-docs-release-1.11/dev/datastream_api.html#fault-tolerance)

[State & Checkpointing](https://ci.apache.org/projects/flink/flink-docs-release-1.11/dev/stream/state/checkpointing.html)  describes how to enable and configure Flink’s checkpointing mechanism.
### Controlling Latency
默认情况下，元素在网络中不是一个接一个传输的，而是使用缓冲机制。缓冲器的大小在Flink配置文件中可以设置。虽然这种方法可以优化吞吐量，但是会导致延迟。为了控制吞吐量和延迟，可以使用`env.setBufferTimeout(timeoutMillis)` on the execution environment (or on individual operators) to set a maximum wait time for the buffers to fill up. 这种情况下，如果时间到了，缓冲器没有满的情况下也会自动发送。默认是100ms.
```java
LocalStreamEnvironment env = StreamExecutionEnvironment.createLocalEnvironment();
env.setBufferTimeout(timeoutMillis);

env.generateSequence(1,10).map(new MyMapper()).setBufferTimeout(timeoutMillis);
```
To maximize throughput, set `setBufferTimeout(-1)` which will remove the timeout and buffers will only be flushed when they are full. To minimize latency, set the timeout to a value close to 0 (for example 5 or 10 ms). A buffer timeout of 0 should be avoided, because it can cause severe performance degradation.
## Debugging
在集群上运行程序前最好在本地调试。
### 使用`LocalStreamEnvironment`
### Collection Data Sources
```java
final StreamExecutionEnvironment env = StreamExecutionEnvironment.createLocalEnvironment();

// Create a DataStream from a list of elements
DataStream<Integer> myInts = env.fromElements(1, 2, 3, 4, 5);

// Create a DataStream from any Java collection
List<Tuple2<String, Integer>> data = ...
DataStream<Tuple2<String, Integer>> myTuples = env.fromCollection(data);

// Create a DataStream from an Iterator
Iterator<Long> longIt = ...
DataStream<Long> myLongs = env.fromCollection(longIt, Long.class);
```
Note: 集合和迭代器必须实现了	`Serializable`
### Iterator Data Sink
```java
import org.apache.flink.streaming.experimental.DataStreamUtils

DataStream<Tuple2<String, Integer>> myResult = ...
Iterator<Tuple2<String, Integer>> myOutput = DataStreamUtils.collect(myResult)
```
## Where to go next?[](https://ci.apache.org/projects/flink/flink-docs-release-1.11/dev/datastream_api.html#where-to-go-next)

-   [Operators](https://ci.apache.org/projects/flink/flink-docs-release-1.11/dev/stream/operators/index.html): Specification of available streaming operators.
-   [Event Time](https://ci.apache.org/projects/flink/flink-docs-release-1.11/dev/event_time.html): Introduction to Flink’s notion of time.
-   [State & Fault Tolerance](https://ci.apache.org/projects/flink/flink-docs-release-1.11/dev/stream/state/index.html): Explanation of how to develop stateful applications.
-   [Connectors](https://ci.apache.org/projects/flink/flink-docs-release-1.11/dev/connectors/index.html): Description of available input and output connectors.
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
