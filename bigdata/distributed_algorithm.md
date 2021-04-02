# 资料
1. [ucsd](https://cseweb.ucsd.edu/classes/sp16/cse291-e/index/lecture_index.html)
# CAP theorem(CAP 定理)
- Consistency: Every read receives the most recent write or an error
- Availability: Every request receives a (non-error) response, without the guarantee that it contains the most recent write
- Partition tolerance: The system continues to operate despite an arbitrary number of messages being dropped (or delayed) by the network between nodes
# Base理论
# 选举算法
## 1. Bully算法
## 2. Ring算法
# 一致性算法(consensus algorithm)
## 1. Paxos(1989)
复杂难懂
## 2. [Zab](https://marcoserafini.github.io/papers/zab.pdf)(before 2009) 
Zab is a crash-recovery atomic broadcast algorithm we designed for the ZooKeeper coordination service
## 3. [Raft](https://raft.github.io/raft.pdf)(2013)
## 时钟同步算法(clock synchronization algorithm)
为了协调物理时钟
## 1. 集中式
使用一个时钟服务器，其他所有的节点的时间都以此时钟服务器为参考，包括：Berkeley Algorithm, Cristian's algorithm等
## 2. 分布式
NTP (Network time protocol)
## 逻辑时钟算法(Logical clock algorithm)
确定事件的顺序 
- Lamport timestamps
- Vector clocks
- Version vectors
- Matrix clocks,
