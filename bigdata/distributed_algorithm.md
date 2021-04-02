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
