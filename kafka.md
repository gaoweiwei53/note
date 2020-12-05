# 1. 概念
- Topic: 为了有条理的管理消息队列；每个topic有分区；在一个broker内，分区中有一个为leader，其他为follower，follower只起备份的作用。
- consumer group: 每一个分区里的消息只能被**同一个消费者组里的同一个消费者**所消费。可以被其他消费者组的消费者消费。
- Zookeeper: 
  - 帮kafka集群存储一些信息
  - 帮助存储消费者消费到的位置信息
- 0.9 版本之前 offset存储在zK中，0.9之后存储本地中

# 2. 安装启动
2.1 配置
- 分布式搭建时，配置zookeeper.connection，不同节点用逗号隔开  

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
