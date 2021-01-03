# 什么是Zookeeper？
## Zookeeper的特点
1) Zookeeper：一个领导者（Leader），多个跟随者（Follower）组成的集群。
2) 集群中只要有半数以上节点存活，Zookeeper集群就能正常服务。
3) 全局数据一致：每个Server保存一份相同的数据副本，Client无论连接到哪个Server，数据都是一致的。
4) 更新请求顺序进行，来自同一个Client的更新请求按其发送顺序依次执行。
5) 数据更新原子性，一次数据更新要么成功，要么失败。
6) 实时性，在一定时间范围内，Client能读到最新数据。
## 选举机制
[参考1](https://zhuanlan.zhihu.com/p/100938553)  
[参考2](https://zhuanlan.zhihu.com/p/97492397)  
[参考3](https://www.jianshu.com/p/2bceacd60b8a)  
[参考4](https://zhuanlan.zhihu.com/p/210999265)  
默认的选举算法叫**Fastleader**
### [Zab: ZooKeeper Atomic Broadcast](https://cwiki.apache.org/confluence/display/ZOOKEEPER/Zab)
Zab是ZooKeeper 原子广播协议(Atomic Broadcast protocol)，Zookeeper使用它来传播leader产生的状态变化。

## 节点类型
- Persistent node
```
create /***
```
- ephemeral node
```
create -e /***
```
- Persistent-sequential node
```
create -s /***
```
- ephemeral-sequential node
```
create -s -e /***
```
