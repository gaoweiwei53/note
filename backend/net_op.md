# net

# Operating System
- [参考1](https://blog.csdn.net/qq_29677867/article/details/91038642)
- [参考2](https://www.cl.cam.ac.uk/teaching/1011/OpSystems/os1a-slides.pdf)
- [Operating Systems: Internals and Design Principles-William Stallings](https://repository.dinus.ac.id/docs/ajar/Operating_System.pdf)
## 1. Processes
### 1.1 线程(Process)
线程是运行中的程序，由其**程序代码**和运行相关的**数据**所组成。一个线程被**process control block**的数据结构唯一标识.

进程的5种状态:**Running**, **Ready**, **Blocked/Waiting**, New, Exit


### 1.2 进程(Thread)
## 2. Concurrency
- 临界区(Critical section): 进程内需要访问共享资源的一段代码，当另一个进程在相应的代码段中时，这段代码不能执
- 死锁(Deadlock): 两个或多个进程无法继续进行，因为每个进程都在等待另一个进程执行某些操作的情况
- 活锁(LiveLock): 两个或多个进程在不做任何有用工作的情况下，不断地改变它们的状态以响应其他进程的变化。
- 竞争条件(Race condition): 在这种情况下，多个线程或进程读写一个共享数据项，最终结果取决于它们执行的相对时间
- 饥饿(): 一个可运行的进程被调度程序无限地忽略的一种情况;虽然它能够前进，但它永远不会被选择。

### 进程互斥算法
- Dekker’s Algorithm
- Peterson’s Algorithm
## 3. Memory Manegement
- Frame: 一种固定长度的主存块。
- Page: 在辅助内存(如磁盘)中的固定长度的数据块。一页数据可以暂时复制到主存的一个Frame中。
- Segment: 在辅助存储器中的一种变长数据块。整个段可以临时复制到主内存的一个可用区域(段)中，或者段可以划分为页，这些页可以单独复制到主内存和分页组合)。
### 内存管理技术
|Technique|Description|Strengths|Weaknesses|
|:--:|:--:|:--:|:--:|
|Fixed Partitioning||||
|Dynamic Partitioning||||
|Simple Paging||||
|Simple Segmentation||||
|Virtual Memory Paging||||
|Virtual Memory Segmentation||||
## 4. Scheduling
## 5. Security
## 6. Distributed Systems
