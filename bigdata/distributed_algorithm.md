# 资料
1. [ucsd](https://cseweb.ucsd.edu/classes/sp16/cse291-e/index/lecture_index.html)
2. [colorado state university](https://www.cs.colostate.edu/~cs551/CourseNotes/)
# CAP theorem(CAP 定理)
- Consistency: Every read receives the most recent write or an error
- Availability: Every request receives a (non-error) response, without the guarantee that it contains the most recent write
- Partition tolerance: The system continues to operate despite an arbitrary number of messages being dropped (or delayed) by the network between nodes
# Base理论
# 一致性
- 强一致性：在任意时刻，从任意不同副本取出的值都是一样的。
- 弱一致性：有时泛指最终一致性，是指在任意时刻，可能由于网络延迟或者设备异常等原因，不同副本中的值可能会不一样，但经过一段时间后，最终会变成一样。
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
有很多版本：
- Basic Paxos
- Multi Paxos
- Fast Paxos

### 假设
#### 节点
- 节点可以任意速度处理
- 节点可能会失败
- 节点失败后可能会重启
- 节点不会说谎破坏协议(不会出现**拜占庭失败**)

#### 网络
- 节点可以给其他任意节点发消息
- 消息发送是异步的
- 消息可能会丢失，乱序或重复
- 消息的发送不会出欺骗(不会出现拜占庭失败)

### 角色介绍：
- Client：系统外部角色，请求发起者。
- Proposer：接受Client请求，向集群提出提议(proposal)。并在冲突发生时，起到冲突调节的作用。
- Acceptor(Voter): 提议投票和接收者，只有在形成法定人数(Quorum)时(一般为majority)时，提议才会最终被接受。
- Learner：提议接收者，backup，备份，对集群一致性没什么影响。

### Safety and liveness
- Validity (or non-triviality): 只有被提议的值可以被选择和learned
- Agreement (or consistency, or safety): 不会有不同的learner learned不同的值
- Termination (or liveness)：If value C has been proposed, then eventually learner L will learn some value 

> Paxos只能保证前两个属性，无法保证**liveness**属性

### 一般的部署方式
常规的部署方式中，每个参与的节点会扮演三个角色Proposer, Acceptor and Learner
### Basic Paxos
Basic Paxos协议的每个“实例”(或“执行”)决定一个单独的输出值
#### 步骤：
1) 阶段1a: Prepare
    - 一个Proposer产生一个叫`Prepare`的消息，`Prepare`消息只含编号值n，不包含要提议的值，编号`n`必须大于这个proposer之前提出的`Prepare`编号。该Proposer将`Prepare`消息发送给Acceptors的一个Quorum
2) 阶段1b：Promise
    - 任何一个Acceptor都会等待接收任意一个Proposer的Prepare消息。如果一个Acceptor收到一个Prepare消息，它会检查n值，这会出现两种情况：
        - 如果n大于此acceptor之前接收到的来自任何Proposer的proposal编号则接受，Acceptor会返回一个`Promise`消息给该Proposer。如果Acceptor在过去接受过一个proposal，那么它必须将该proposal number和值包含在回复给Proposer的消息中。(所有历史的proposal的number和value吗？)
        - 否则(n小于等于...)Acceptor会忽略接收到的proposal，虽然此时Acceptor没必要回复任何消息，但是为了优化，它会发送一个拒绝消息`Nack`给Proposer
3) 阶段2a：Accept
    - 如果一个Proposer接收到了来自 Acceptors的Quorum的大部分Promise消息，它需要在proposal里设置一个值`v`. 如果任何一个Acceptor之前接受过任何proposal，它会将这些proposal的value发送给该proposer，该proposer现在必须将当前proposal的`v`设置成所有Acceptors汇报中的最大的proposal number, 记为`z`。如果到目前为止Acceptors中没有一个曾接受过propoal，那么该Proposer就可以选择它原始想设置的值，记为`x`. 然后 Proposer会发送*Accept*消息`(n,v)`给Accepters中的一个Quorum 。*Accept*消息可能为`(n, v=z)`或`(n, v=x)`，`n`就是Prepare阶段设置的n. *Accept*消息可理解为“请求接收proposal吧！”
4) 阶段2b: Accepted
    - 如果一个acceptor接收到*Accept*消息`(n,v)`，它必须接受该消息，当且仅当它没有promise过proposal number大于n的proposal.如果它没有promise过proposal number大于n的proposal，它将注册v值为已接受的value, 然后会发送一个`Accepted `消息给这个Proposer和每个Learner()

潜在问题：活锁(liveness)或dueling，角色太多，难实现、效率低(2次RPC)
#### Multi Paxos
新概念，Leader：唯一的propser，所有请求都需经过此Leader(唯一的proposer)

开始集群会选举出一个节点作为leader

## 2. [Zab](https://marcoserafini.github.io/papers/zab.pdf)(before 2009) 
Zab is a crash-recovery atomic broadcast algorithm we designed for the ZooKeeper coordination service
## 3. [Raft](https://raft.github.io/raft.pdf)(2013)
划分为三个子问题：
- Leader Election
- Log Replication
- Safety
### 重要角色
- Leader
- Follower
- Candidate
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
两阶段提交是一种原子事务提交协议
在分布式系统中，为了让每个节点都能够感知到其他节点的事务执行状况，需要引入一个中心节点来统一处理所有节点的执行逻辑，这个中心节点叫做协调者（coordinator），被中心节点调度的其他业务节点叫做参与者（participant）。2PC将分布式事务分成了两个阶段，两个阶段分别为提交请求（投票）和提交（执行）。协调者根据参与者的响应来决定是否需要真正地执行事务，具体流程如下。

缺点：2PC的最大缺点是它是一个阻塞协议。如果协调者永久失效，参与者将永远无法解析其事务:在参与者向协调者发送了协议消息后，参与者将阻塞，直到接收到提交或回滚。
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
## 拜占庭错误(Byzantine fault, Byzantine generals problem)
分布式系统中的某些节点可能出错而发送错误的信息，使得不同的节点基于一致性策略得出不同的结论，从而破坏系统的一致性。
### 解决算法
Byzantine fault tolerance (BFT)
## [租约机制(lease mechanism)](https://blog.csdn.net/kevinfankai/article/details/4024937)
租约机制是一种维护缓存一致性的方法。缓存一致性就是客户端总能读到最新的数据。使用缓存后有可能服务器端的数据已经被修改，但客户端仍然从缓存中读取陈旧的数据。

服务器给予客户端在 一定期限内可以 控制修改操作的 权力。如果服务器要修改数据，首先要征求拥有这块数据的租约的客户端的同意，之后才可以修改。客户端从服务器读取数据时往往就同时获取租约，在租约期限 内，如果没有收到服务器的修改请求，就可以保证当前缓存中的内容就是最新的。如果在租约期限内收到了修改数据的请求并且同意了，就需要清空缓存。在租约过 期以后，客户端如果还要从缓存读取数据，就必须重新获取租约，我们称这个操作为“ 续约”。

### [HDFS租约机制](https://blog.csdn.net/Androidlushangderen/article/details/52850349?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.control&dist_request_id=&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.control)
在HDFS中，当每次客户端用户往某个文件中写入数据的时候，为了保持数据的一致性，此时其它客户端程序是不允许向此文件同时写入数据的。

## Quorums
Quorum机制是在分布式系统中用来保证一致性操作的方法。它具体指的是分布式事务被允许执行操作而必须获得的最小投票数。

Quorum机制可用在副本控制和提交协议中：
### 1. 提交协议
在分布式数据库系统中，一个事务可以在多个节点上执行它的操作。由于原子性要求每个分布式事务都是原子的，因此事务在每个节点上必须具有相同的操作(提交或中止)。在网络分区的情况下，节点被分区，分区之间可能无法通信。这就需要基于quorum的技术。其基本思想是，如果大多数站点投票执行事务，则执行事务。

每个节点被分配一个投票`Vi`, 假设系统中总的投票数为`V`, 撤销和提交的Quorums分别是`Va`和`Vc`。在提交的时候遵守以下规则：
1) `Va` + `Vc` > V, 0 < `Va`, `Vc` <= V
2) 在事务提交之前，必须获得quorum `Vc`. 至少有一个节点准备提交，0或多个节点等待 >= `Vc`
3) 在事务撤销之前，必须获得撤销quorum `Va`. 至少有一个节点准备撤销，0或多个节点等待 >= `Vc`

第一条规则保证事务不会同时被执行提交和撤销，后两条规则指的事务提交和撤销前必须获得相应的quorum
### 2. 副本控制
在具有副本的数据库中，一个数据对象在几个节点上都有副本。为了确保可串行性，不应该允许两个事务同时读写一个数据项。对此可以使用基于quorum的副本控制协议来确保两个事务不会并发地读写数据项的两个副本。

每一个副本的数据项都被分配一个投票。每个操作当读或写的时候都必须获得read quorum(`Vr`)或write quorum(`Vw`)。给定一个数据项，它共有`V`个投票， 则必须遵守以下规则：
1) `Vr` + `Vw` > `V`
2) Vw > V / 2

第一条规则确保一个数据项不会被两个事务并发地读写。此外，它确保read quorum 包含至少一个具有数据项最新版本的站点。第二条规则确保来自两个事务的两个写操作不能并发地发生在同一数据项上。这两个规则确保维护了单副本的可串行性。

Quorum 机制无法保证强一致性，也就是无法实现任何时刻任何用户或节点都可以读到最近一次成功提交的副本数据。
