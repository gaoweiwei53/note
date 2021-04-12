# 资料
1. [ucsd](https://cseweb.ucsd.edu/classes/sp16/cse291-e/index/lecture_index.html)
2. [colorado state university](https://www.cs.colostate.edu/~cs551/CourseNotes/)
# CAP theorem(CAP 定理)
- Consistency: Every read receives the most recent write or an error
- Availability: Every request receives a (non-error) response, without the guarantee that it contains the most recent write
- Partition tolerance: The system continues to operate despite an arbitrary number of messages being dropped (or delayed) by the network between nodes
# Base理论
# 选举算法
## 1. [Bully算法](https://www.cs.colostate.edu/~cs551/CourseNotes/Synchronization/BullyExample.html)
The algorithm uses the following message types:
- Election Message: Sent to announce election.
- Answer (Alive) Message: Responds to the Election message.
- Coordinator (Victory) Message: Sent by winner of the election to announce victory.

When a process P recovers from failure, or the failure detector indicates that the current coordinator has failed, P performs the following actions:

1) If P has the highest process ID, it sends a Victory message to all other processes and becomes the new Coordinator. Otherwise, P broadcasts an Election message to all other 2) processes with higher process IDs than itself.
3) If P receives no Answer after sending an Election message, then it broadcasts a Victory message to all other processes and becomes the Coordinator.
4) If P receives an Answer from a process with a higher ID, it sends no further messages for this election and waits for a Victory message. (If there is no Victory message after a period of time, it restarts the process at the beginning.)
5) If P receives an Election message from another process with a lower ID it sends an Answer message back and starts the election process at the beginning, by sending an Election message to higher-numbered processes.
6) If P receives a Coordinator message, it treats the sender as the coordinator.
## 2. [Ring算法](https://www.cs.colostate.edu/~cs551/CourseNotes/Synchronization/RingElectExample.html)
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
## 两阶段提交
在分布式系统中，为了让每个节点都能够感知到其他节点的事务执行状况，需要引入一个中心节点来统一处理所有节点的执行逻辑，这个中心节点叫做协调者（coordinator），被中心节点调度的其他业务节点叫做参与者（participant）。2PC将分布式事务分成了两个阶段，两个阶段分别为提交请求（投票）和提交（执行）。协调者根据参与者的响应来决定是否需要真正地执行事务，具体流程如下。

#### 1. 提交请求（投票）阶段
协调者向所有参与者发送prepare请求与事务内容，询问是否可以准备事务提交，并等待参与者的响应。
参与者执行事务中包含的操作，并记录undo日志（用于回滚）和redo日志（用于重放），但不真正提交。
参与者向协调者返回事务操作的执行结果，执行成功返回yes，否则返回no。
#### 2. 提交（执行）阶段
分为成功与失败两种情况。
1) 若所有参与者都返回yes，说明事务可以提交：
    - 协调者向所有参与者发送commit请求。
    - 参与者收到commit请求后，将事务真正地提交上去，并释放占用的事务资源，并向协调者返回ack。
    - 协调者收到所有参与者的ack消息，事务成功完成。
    - 若有参与者返回no或者超时未返回，说明事务中断，需要回滚：

2) 协调者向所有参与者发送rollback请求。
    - 参与者收到rollback请求后，根据undo日志回滚到事务执行前的状态，释放占用的事务资源，并向协调者返回ack。
    - 协调者收到所有参与者的ack消息，事务回滚完成。
### 2PC的缺点
1) 协调者存在单点问题。如果协调者挂了，整个2PC逻辑就彻底不能运行。
2) 执行过程是完全同步的。各参与者在等待其他参与者响应的过程中都处于阻塞状态，大并发下有性能问题。
3) 仍然存在不一致风险。如果由于网络异常等意外导致只有部分参与者收到了commit请求，就会造成部分参与者提交了事务而其他参与者未提交的情况。
