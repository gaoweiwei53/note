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
