# 1. 概念
- Partition: 可保证负载均衡和提高并发度
- Replication: 每一个分区有副本，起备份的作用，同一个分区的一个副本不会存储在同一个broker上
- Topic: 为了有条理的管理消息队列；每个topic有分区；在一个broker内，分区中有一个为leader，其他为follower副本，follower副本只起备份的作用。
- consumer group: 每一个分区里的消息只能被**同一个消费者组里的同一个消费者**所消费。可以被其他消费者组的消费者消费。
- Zookeeper: 
  - 帮kafka集群存储一些信息
  - 帮助存储消费者消费到的位置信息
- 0.9 版本之前 offset存储在zK中，0.9之后存储本地中
- 发布/订阅可分为两种模式
  - 主动拉取: 消费者可自己控制消费速度
  - 主动推送

# 2. 安装启动
2.1 配置
- 分布式搭建时，配置zookeeper.connection，不同节点用逗号隔开
- 副本数要求小于broker数量

2.2 群起脚本
```bash
#!/bin/bash
case $1 in
"start"){
  for i in hadoop102 hadoop103 hadoop104
  do
    echo "*******$i**********"
    ssh $i "启动命令"
  done
};;
"stop"){
  for i in hadoop102 hadoop103 hadoop104
  do
    echo "*******$i**********"
    ssh $i "启动命令"
  done
};;
esac
```
```bash
chmod 777 start_stop.sh
start_stop.sh start
start_stop.sh stop
```
# 3. 数据可靠性保证
### 1) 副本数据同步策略
全部完成同步，才发送a c k
### 2) ISR
采用第二种方案之后，设想以下情景：leader收到数据，所有follower都开始同步数据，但有一个follower，因为某种故障，迟迟不能与leader进行同步，那leader就要一直等下去，直到它完成同步，才能发送ack。这个问题怎么解决呢？
Leader维护了一个动态的in-sync replica set (I S R)，意为和leader保持同步的follower集合。
当ISR中的follower完成数据的同步之后，leader就会给follower发送a c k。如果follower长时间未向leader同步数据，则该follower将被踢出I S R，该时间阈值由replica.lag.time.max.ms参数设定。Leader发生故障之后，就会从I S R中选举新的leader
### 3）ack应答机制
对于某些不太重要的数据，对数据的可靠性要求不是很高，能够容忍数据的少量丢失，所以没必要等ISR中的follower全部接收成功。
所以Kafka为用户提供了三种可靠性级别，用户根据对可靠性和延迟的要求进行权衡，选择以下的配置。
ack：
0: producer不等待broker的ack，会丢失数据
1：producer只等待leader落盘后返回ack，会丢失数据
-1：producer等待leader和**ISR中的**所有follower落盘后返回ack，会重复数据
### 4) 故障处理细节
**LEO**(log end offset): 每个副本中最后一个offset，每个副本中最大的offset

**HW**(high watermark): 所有副本中最小的LEO，消费者能见到的最大offset，ISR队列中最小的LEO
### 5) exactly-once
**幂等性**：指producer不论向server发送了多少重复数据，server端只会持久化一条，幂等性结合at least once就构成了Kafka的Exactly Once语义

# 4. 消费者
## 4.1 消费方式
采用pull方式
- 优点是可根据消费者的消费能力以适当的速率消费信息
- 缺点是kafka没有数据时，会陷入循环，返回空数据，解决：设置时长timeout，没有数据时，等一段timeout时间再返回
## 4.2 分区分配策略 (不是很懂)
Kafka有两种分配策略
- RoundRobin

按照消费者组划分

要保证一个消费者组只消费同一个主题
- Range
按照主题划分

默认的是Range

## 4.3 offset维护
consumer需要实时记录自己消费到哪个offset，以便在故障恢复后继续消费，按 group+ Topic + partition 决定唯一的offset

# 5. Kafka高效读写数据
## 5.1 为啥快
[参考1](https://www.cnblogs.com/yanggb/p/11063942.html)

[参考2](https://kafka.apache.org/documentation/#maximizingefficiency)
### 1. 顺序写磁盘
写的过程中一直追加到文件的末端，，省去了大量寻址时间。Kafka是不会删除数据的，它只会把所有的数据都保留下来，每个消费者（Consumer）对每个Topic都有一个offset用来表示读取到了第几条数据。

如果数据完全不删除，那么肯定会导致硬盘爆满，所以Kafka提供了两种策略来删除数据，一是基于时间，二是基于Partition文件大小。具体配置可以参看它的配置文档。

系统会产生两种效率不高的问题：**I/O操作太多**和**字节拷贝(byte copy)**

### 2. 批处理
解决**I/O操作太多**问题的方法是将消息进行分组，相比每次只发送单个消息，将消息分组可以减少网络的开销。并且服务器也会将消息成块地追加到日志记录中，此外，消费者也是按批消费消息。

解决**字节拷贝(byte copy)** 问题地方是在producer, broker, consumer之间使用相同的标准二进制文件格式，这样消息块在它们之间传送就不用更改了。

由broker维护的消息日志本身就是一个文件目录，每个文件都由一系列消息集填充，这些消息集以producer和consumer使用的相同格式写入磁盘。维护这种通用格式可以优化最重要的操作:持久日志块的网络传输。现代的unix操作系统提供了一个高度优化的代码路径，用于将数据从页缓存(pagecache)传输到socket;在Linux中，这是通过**sendfile system call**完成的。

### 3. [零拷贝](https://developer.ibm.com/articles/j-zerocopy)
传统模式下从硬盘读取文件：
- 1. 数据通过DMA(direct memory access)方式被复制到内核(Kernel Context)的地址空间缓冲区/**pagecache**/Reader Buffer. User Mode --> Context Mode
- 2. 数据从Reader Buffer复制到到User Buffer. Context Mode --> User Mode
- 3. 数据被第三次复制，从用户缓冲区复制到内核地址空间缓冲区，这一次数据放入到了Socket Buffer. User Mode --> Context Mode
- 4. 第四次复制，通过DMA 方式将数据从kernel buffer 复制到协议引擎。Context Mode --> User Mode

![图1](https://developer.ibm.com/developer/default/articles/j-zerocopy/images/figure1.gif)
> 复制操作需要在用户模式和内核模式之间进行四次上下文切换，数据在操作完成之前复制四次

Zero-Copy:

![图2](https://developer.ibm.com/developer/default/articles/j-zerocopy/images/figure5.gif)

- 1. 数据通过DMA(direct memory access)方式被复制到内核(Kernel Context)的地址空间缓冲区/Reader Buffer. User Mode --> Context Mode
- 2. 数据不直接复制到Socket Buffer，而是将`descriptor`追加到Socket Buffer, `decriptor`记录了数据的位置和长度。DMA引擎直接将数据从kernel buffer复制到协议引擎
> 数据被复制了两次，模式切换两次

### 2. MMFiles（Memory Mapped Files）
即便是顺序写入磁盘，磁盘的访问速度还是不可能追上内存的。所以Kafka的数据并不是实时的写入硬盘，它充分利用了现代操作系统的分页存储来利用内存，以此来提高I/O效率。Memory Mapped Files（后面简称MMAP）也被翻译成内存映射文件，在64位操作系统中一般可以表示20G的数据文件。它的工作原理是直接利用操作系统的Page来实现文件到物理内存的直接映射。完成映射之后，你对物理内存的操作会被同步到硬盘上（操作系统在适当的时候）

缺陷：不可靠，因为写到MMAP中的数据并没有被真正地写入到硬盘中，操作系统会在程序主动调用flush命令的时候才会把数据真正地写入到硬盘中。Kafka提供了一个参数prducer.type来控制是不是主动flush，如果Kafka写入到MMAP之后就立即flush然后再返回Producer，就叫做同步（sync）；如果Kafka写入到MMAP之后立即返回Producer不调用flush，就叫做异步（async）

### 4. NIO网络通信
### 5. PageCache
### 6. 消息批量压缩
在很多情况下，系统的瓶颈不是CPU或磁盘，而是网络IO，对于需要在广域网上的数据中心之间发送消息的数据流水线尤其如此。进行数据压缩会消耗少量的CPU资源,不过对于kafka而言,网络IO更应该需要考虑。
- 如果每个消息都压缩，但是压缩率相对很低，所以Kafka使用了批量压缩，即将多个消息一起压缩而不是单个消息压缩。
- Kafka将消息批量地压缩，然后同样以压缩的形式传输，并且在日志中也保持压缩格式，直到被消费者解压缩。
- Kafka支持多种压缩协议，包括Gzip和Snappy压缩协议。
## 5.2 Zookeeper的作用
Kafka集群中有一个broker会被选为Controller，Controller的管理工作依赖于Zookeeper

# 6. Kakfa事务
##  6.1 Producer事务
0.11版本后引入事务支持。事务支持可以保证Kafka在Exactly Once语义的基础上，生产和消费可以跨分区和会话，要么全部成功，要么全部失败。
引入全局唯一的transaction ID
## 6.2 Consumer事务
很少关注




