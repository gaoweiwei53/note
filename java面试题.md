# Java
## 语法基础
#### 1. Java基本数据类型几个？
Java提供了八种基本类型。六种数字类型（四个整数型，两个浮点型），一种字符类型，还有一种布尔型。
- byte   1字节
- short  2字节
- int    4字节
- long   8字节
- float  4字节
- double 8字节
- char   2字节
- boolen 表示1位的信息
#### 2. StringBuffer和StringBuilder的区别？
和String类不同的是，StringBuffer 和 StringBuilder 类的对象能够被多次的修改，并且不产生新的未使用对象。
- StringBuffer 和 StringBuilder 长度可变

- StringBuffer 线程安全 StringBuilder 线程不安全

- StringBuilder 速度快，推荐使用
#### 3. JDK 和 JRE 有什么区别？
JRE 是 java运行时(java runtime environment), 包含了java虚拟机，java基础类库，是java程序运行所必须的环境。

JDK是java开发工具，是开发java应用的工具包，其包括jre，还有其他开发java应用的工具，例如javac编译器
#### 4. equals 和 ==的区别？
- equals()比较两个对象的内容
- 对于基本类型，比较的是值，对于对象，比较的是对象在内存中的地址。
####  5. hashcode
- hashCode()相等的两个对象他们的equal()不一定相等。
- equal()相等的两个对象他们的hashCode()一定相等。
#### 6. final修饰符的作用
 final修饰符用来修饰类、方法和变量，final 修饰的类不能够被继承，修饰的方法不能被继承类重写，修饰的变量为常量，是不可修改的。
#### 7. Math.round(-1.5)的值
等于 -1, round()返回四舍五入，负.5小数返回较大整数，如-1.5返回-1，1.5返回2
#### 8. String str="i"与 String str=new String("i")一样吗？

不一样，因为内存的分配方式不一样。String str = "i"的方式，Java虚拟机会将其分配到常量池中，而常量池中没有重复的元素；String str = new String("i")则会被分到堆内存中。
#### 9. 怎么反转字符串
将字符串转为char数组或者使用StringBuilder类的reverse()函数
#### 10. 抽象类必须有抽象方法吗？
不，
- 抽象类不能被实例化(初学者很容易犯的错)，如果被实例化，就会报错，编译无法通过。只有抽象类的非抽象子类可以创建对象。
- 抽象类中不一定包含抽象方法，但是有抽象方法的类必定是抽象类。
- 抽象类中的抽象方法只是声明，不包含方法体，就是不给出方法的具体实现也就是方法的具体功能。
- 构造方法，类方法（用 static 修饰的方法）不能声明为抽象方法。
- 抽象类的子类必须给出抽象类中的抽象方法的具体实现，除非该子类也是抽象类
#### 11. java的I/O流分为几种？
- 按流向分类：
    - 输入流
    - 输出流
- 按处理数据不同分类：
    - 字节流：二进制，可以处理一切文件，包括：纯文本、doc、音频、视频等。
    - 字符流：文本文件，只能处理纯文本。
- 按功能不同分类：
    - 节点流：包裹源头。FileInputStream/FileOutputStream FileReader/FileWriter
    - 处理流：增强功能，提高性能。BufferInputStream/BufferOutputStream BufferReader/BufferWriter

#### 12. BIO, NIO, AIO有什么区别？
- Java BIO  
Block IO，客户端有连接请求时服务器端就需要启动一个线程进行处理，如果这个连接不做任何事情会造成不必要的线程开销，当然可以通过线程池机制改善。面向流
- Java NIO  
No block IO，客户端发送的连接请求都会注册到多路复用器上，多路复用器(Selector)轮询到连接有I/O请求时才启动一个线程进行处理。面向缓冲区。
|||
- Java AIO(NIO.2)  
Asynchronous non-blocking IO, 客户端的I/O请求都是由OS先完成了再通知服务器应用去启动线程进行处理

##### BIO和NIO的比较
* BIO 以流的方式处理数据,而 NIO 以块的方式处理数据,块 I/O 的效率比流 I/O 高很多
* BIO 是阻塞的，NIO 则是非阻塞的
*  BIO 基于字节流和字符流进行操作，而 NIO 基于 Channel(通道)和 Buffer(缓冲区)进行操作，数据总是从通道
  读取到缓冲区中，或者从缓冲区写入到通道中。Selector(选择器)用于监听多个通道的事件（比如：连接请求，数据到达等），因此使用单个线程就可以监听多个客户端通道

| NIO                       | BIO                 |
| ------------------------- | ------------------- |
| 面向缓冲区（Buffer）      | 面向流（Stream）    |
| 非阻塞（Non Blocking IO） | 阻塞IO(Blocking IO) |
| 选择器（Selectors）       |                     |
#### 13. 说说对面向对象的理解？
#### 14. 说说java8 java9 java11的新特性？函数式接口
- 默认方法：Java 8允许我们给接口添加一个非抽象的方法实现，只需要使用 default关键字即可
- lambda表达式
- Stream API
#### 15. `instanceof`的作用？
a instanceof A: 判断a是否位类A的实例
#### 16. 说说红黑树？
二叉搜索树 --> 平衡树 --> 

（1）每个节点或者是黑色，或者是红色。
（2）根节点是黑色。
（3）每个叶子节点（NIL）是黑色。 [注意：这里叶子节点，是指为空(NIL或NULL)的叶子节点！]
（4）如果一个节点是红色的，则它的子节点必须是黑色的。
（5）从一个节点到该节点的子孙节点的所有路径上包含相同数目的黑节点。
#### 17. 什么是函数式接口
函数式接口是只包含一个抽象方法声明的接口。`java.lang.Runnable` 就是一种函数式接口，在 `Runnable` 接口中只声明了一个方法 `void run()`. 函数式接口都可用用lambda表达式来简化书写。
#### 18. Stream
在 Java 8 中, 集合接口有两个方法来生成流：
- stream() − 为集合创建串行流。
- parallelStream() − 为集合创建并行流。
## 集合
#### 1. Collection 和 Collections 有什么区别？
- Collection是一个集合接口。它提供了对集合对象进行基本操作的通用方法。
- Collections 是一个包装类。它包含有各种有关集合操作的静态方法。

#### 2. HashMap 和 Hashtable 有什么区别？
- Hashtable
    - key和value为非null 
    - Hashtable是同步的，也就说时线程安全的
    - 如果不需要线程安全，推荐使用`HashMap`
    - 如果需要线程安全高并发，推荐使用`java.util.concurrent.ConcurrentHashMap`
    - ConcurrentHashMap也是线程安全的，但性能比HashTable好很多，HashTable是锁整个Map对象，而ConcurrentHashMap是锁Map的部分结构

- Hashmap
    - 允许key和value的值为null，不过只能允许一个bull key
    - 不是同步的

#### 3. 如何决定使用 HashMap 还是 TreeMap？
HashMap通过hashcode对其内容进行快速查找，而 TreeMap中所有的元素都保持着某种固定的顺序，如果需要得到一个有序的结果你就应该使用TreeMap（HashMap中元素的排列顺序是不固定的）。
- HashMap：基于哈希表实现，适用于在Map中插入、删除和定位元素。
- TreeMap：基于**红黑树**实现，适用于按自然顺序或自定义顺序遍历键(key)
- HashMap通常比TreeMap快一点(树和哈希表的数据结构使然)，建议多使用HashMap，在需要排序的Map时候才用TreeMap
- HashMap和TreeMap都是非同步的

#### 4. 说一下 HashMap 的实现原理？
- load factor: a=n/m 其中n 为关键字个数，m为表长。表示Hsah表中元素的填满的程度.若:加载因子越大,填满的元素越多,好处是,空间利用率高了,但:冲突的机会加大了.反之,加载因子越小,填满的元素越少,好处是:冲突的机会减小了,但空间浪费多了。冲突的机会越大,则查找的成本越高.反之,查找的成本越小.因而,查找时间就越小.
- rehash: 在散列，当load factor达到阈值时，散列表进行扩容。

HashMap是通过Hash表实现的，该Hash表是由数组和单链表构成的。当不发生hash冲突时，将数据放在对应的数组位置，当发生hash冲突时，使用单链表结构，将新加入的数据作为到链表的下一个节点。
#### 5. 说一下 HashSet 的实现原理？
HashSet的是基于HashMap实现的，将set需要存储的值放在map的key里的，value是一个空的object对象(PRESENT).
#### 6. ArrayList 和 LinkedList 的区别是什么？
ArrayList是通过可增长容量的数组实现的，LinkedList是通过双链表实现的。
- ArrayList：查找的时候性能好
- LinkedList：添加删除元素的时候性能好

#### 7. 如何实现数组和 List 之间的转换？
- 数组转List
    - 通过 Arrays.asList(strArray) 方式,将数组转换List后，不能对List增删，只能查改，否则抛异常。
    - 通过ArrayList的构造器，将Arrays.asList(arr)的返回值转为ArrayList。  
    - 通过集合工具类Collections.addAll()方法(最高效)。
    ```java
    String[] arr={"王昭君","妲己","后羿","鲁班","程咬金"};
    ArrayList<String> list3=new ArrayList<String>();
    Collections.addAll(list3, arr);
    ```
- List转数组(方法)
    - list.toArray(new String[list.size()])(常用) 

#### 8. ArrayList 和 Vector 的区别是什么？
都是基于数组实现的
- Vector比ArrayList先存在。Vector是同步的，Vector的对象是线程安全的；ArrayList是异步的，ArrayList的对象不是线程安全的。同步影响执行效率，所以ArrayList比Vector性能好。
- ArrayList和Vector都有一个初始的容量大小，当存储的空间不够时，需要增加存储空间，Vector默认增长原来的一倍，而ArrayList是原来的0.5倍。ArrayList与Vector都可以设置初始的空间大小，Vector还可以设置增长的空间大小。
#### 9. Array 和 ArrayList 有何区别？
- Array可以包含基本类型和对象类型，ArrayList只能包含对象类型。
- Array大小是固定的，ArrayList的大小是动态变化的。
- ArrayList提供了更多的方法和特性，比如：addAll()，removeAll()，iterator()等等。

#### 10. 迭代器 Iterator 是什么？怎么使用
 - Iterator是一个接口，定义了`hasNext()`, `Next()`, `remove()`等方法

集合类一般在类中定义定义一个内部类实现迭代器接口，同时定义一个方法用来返回该迭代器类的对象。使用的时候调用该方法，即可获得该集合的迭代器。
#### 11. Iterator和ListIterator的区别
- 使用范围不同，Iterator可以应用于所有的集合，Set、List和Map和这些集合的子类型。而ListIterator只能用于List及其子类型。
- ListIterator有add方法，可以向List中添加对象，而Iterator不能。
- ListIterator和Iterator都有hasNext()和next()方法，可以实现顺序向后遍历，但是ListIterator有hasPrevious()和previous()方法，可以实现逆向（顺序向前）遍历。Iterator不可以。
- ListIterator可以定位当前索引的位置，nextIndex()和previousIndex()可以实现。Iterator没有此功能。
- 都可实现删除操作，但是ListIterator可以实现对象的修改，set()方法可以实现。Iterator仅能遍历，不能修改。
#### 12. 怎么确保一个集合不被修改
通过Collections工具类的静态方法unmodifiableCollection（list）, 该静态方法内部返回了Collections的静态内部类UnmodifiableCollection对象，同样实现了Collection集合的方法，只不过在比如add、remove等修改的方法中直接抛出UnsupportedOperationException()异常，因此实现了集合不能修改的功能。

## 并发编程
#### 1. 并发和并行的区别？
- 并行是同一时刻，多个任务同时执行
- 并行是在一个时间段内，多个任务可交替执行

> 单核CPU内，任务只能并发执行，无法并行执行，多核CPU内，每个核可并行执行任务

#### 2. 进程和线程
- 进程：是正在运行的程序的实例。
- 线程：包含在进程之中，是进程中的实际执行实体，是操作系统能够进行运算调度的最小单位。

不同的进程使用独立的地址空间和资源，不同的线程共享地址空间和系统资源
#### 3. java线程的状态有几种？
#### 4. 守护线程是什么？使用场景？
守护线程在JVM中是一个低优先级的线程，它在后台运行，例如垃圾收集(gc)等任务，当所有用户线程完成执行时结束时，守护线程本身正在运行也不会阻止JVM退出

可以使用`setDaemon(boolean)`方法在线程启动之前更改线程守护进程属性

事实上，User Thread（用户线程）和Daemon Thread（守护线程）从本质上来说并没有什么区别，唯一的不同之处：如果用户线程已经全部退出运行了，只剩下守护线程存在了，虚拟机也就退出了。
#### 5. java线程有哪几种状态？
6种
- New
- Runnable
- Blocked
- Waiting 
- Timed Waiting 
- Terminated

Waiting无限时间的等待 Runnable -> waiting: `wait()`, `join()` 切回Runnable: `notify()`, `notifyAll()`

Timed Waiting 以一个具体时间的等待  Runnable -> waiting: `wait(long)`, `join(long)`, `sleep(long)`, 切回Runnable: `notify()`, `notifyAll()`
#### 6. 线程的方法
- `sleep()`: 休眠当前线程，释放 CPU 资源，不释放对象锁，休眠时间到自动苏醒继续执行
- `wait()`: 当前线程释放CPU资源和monitor锁，进入阻塞状态。
- `notify()`: 唤醒被wait的一个线程。如果有多个线程被wait，就唤醒优先级高的。线程被唤醒后，需要竞争锁，获取到锁之后再继续执行。
- `notifyAll()`: 会唤醒所有被wait的线程
- `join()`: 如果某个线程调用另一个线程的`join()`方法`t.join()`，那么当前线程将被挂起，直到目标线程t结束才恢复
- `yield()`: 线程告诉调度器，表示愿意让出对CPU的当前使用权，但是调度器可以自由忽略这个提示。 **不常用**

[代码](#线程方法)

说明
- `wait()`, `notify()`, `notifyAll()`必须使用在同步代码块或同步方法种
- `wait()`, `notify()`, `notifyAll()`三个方法的调用者必须是同步代码块或同步方法中的同步monitor
- `wait()`, `notify()`, `notifyAll()`定义在Object类中

`sleep(long mills)`：让出CPU资源，但是不会释放锁资源。
`wait()`：让出CPU资源和锁资源。
#### 7. 创建线程的方式
1) 通过继承Thread类实现,重写`run()`方法，缺点：单继承的限制----无法继承其它父类; 多个线程之间无法共享该线程类的实例变量。
2) 实现Runnable接口，实现`run()`方法，较继承Thread类，避免单继承的局限性，适合资源共享。
3) 使用Callable，接口中要覆盖的方法是`call()`, 此方法可以抛异常,而前两种不能 而且此方法可以有返回值

#### 8. Runnable和Callable的区别
Runnable和Callable的区别是，
- Callable规定的方法是call(),Runnable规定的方法是run().
- Callable的`call()`可以有返回值并可以抛出异常，而Runnable的`run()`方法没有返回值，不抛出异常。
#### 9. `start()`和`run()`方法的区别？
`start()`方法用来启动该新的线程，`run()`方法会在该线程里执行。如果不调用`start()`, 直接调用`run()`, 该线程不会被启动，`run()`方法会被调用线程即main线程当作普通的类方法执行。
#### 10. 创建线程池有几种方式？
有两种：
1) `ThreadPoolExecutor`
    ```java
    ExecutorService service = new ThreadPoolExecutor(8,8, more args here...);
    ```
2) `Executors`工厂, 其实调用静态方法最终返回的也是`ThreadPoolExecutor`对象，**建议使用**
    ```java
    ExecutorService service = Executors.newFixedThreadPool(8); // 固定数量的线程
    ExecutorService service = Executors.newCachedThreadPool(8); // 适合执行许多短时间存活的异步任务
    ExecutorService service = Executors.newScheduledThreadPool(8); //
    ExecutorService service = Executors.newSingleThreadExecutor(8);
    ```
#### 11. 线程池有哪些状态？
|状态|内容|
|-|-|
|**Running**|接收新的任务，处理队列任务|
|**Shutdown**|不接收新的任务，但处理队列任务|
|**Stop**|不接收新的任务，不处理队列任务|
|**Tidying**|所有任务都已终止，`workCount`为0，线程将转为Tidying状态，并会执行`terminated()`钩子方法|
|**Terminated**|`terminated()`方法执行完成后|

- RUNNING -> SHUTDOWN  调用`shutdown()`
- (RUNNING or SHUTDOWN) -> STOP 调用`shutdownNow()`
- SHUTDOWN -> TIDYING 队列和线程池都为空
- STOP -> TIDYING 线程池为空
- TIDYING -> TERMINATED `terminated()`方法执行完成后

#### 12. 线程池中 submit()和 execute()方法有什么区别？
两个方法都可以向线程池提交任务
- `execute()`只能接收`Runnable`, `submit()`可接收`Runnable`或`Callable`
- `execute()`没有返回值, 它定义在`Executor`接口中; 而 `submit()` 有返回值, 返回持有计算结果的 `Future` 对象, 它定义在`ExecutorService`接口中

> `ExecutorService`接口继承了`Executor`接口
#### 13. 在 java 程序中怎么保证多线程的运行安全？
[答案](https://blog.csdn.net/meism5/article/details/90266334)
#### 14. 多线程锁的升级原理是什么？
#### 15. 什么是死锁？怎么防止死锁？
#### 16. synchronized 和 volatile 的区别是什么？
`synchronized` 关键字声明的方法同一时间只能被一个线程访问。

`volatile` 修饰的成员变量在每次被线程访问时，都强制从共享内存中重新读取该成员变量的值。而且，当成员变量发生变化时，会强制线程将变化值回写到共享内存。这样在任何时刻，两个不同的线程总是看到某个成员变量的同一个值。
#### 17. 说一下 synchronized 底层实现原理？
#### 18. synchronized 和 Lock 有什么区别？
#### 深拷贝和浅拷贝区别是什么？
拷贝分为引用拷贝和对象拷贝，深拷贝和浅拷贝都属于对象拷贝。不同的是如果拷贝对象里有字段引用了其他对象，浅拷贝只会拷贝该字段的引用，而深拷贝会将该字段引用的对象也进行拷贝。

#### java中怎么实现深拷贝？   

`clone()`实现的是浅克隆。要想实现深克隆有两种方法
- 将原始拷贝对象类和字段引用的对象类都实现Cloneable接口
- 实现Serializable接口，所有的引用类型的成员变量也要实现Serializable，但是只需要复制的类重写chone()方法即可。(**推荐使用**)
#### 怎么实现动态代理？
## 异常
#### throw 和 throws 的区别？
- 作用不同：？
- throw位于方法体内部，throws写在方法声明处，方法参数列表后面。
- throw抛出一个异常对象，而且只能是一个，throws后面跟异常类，可以有多个。
#### final、finally、finalize 有什么区别？
- final是修饰符
- finally 用在 try catch后，里面的代码一定会执行
- finalize方法来自于java.lang.Object，用于回收资源。
#### try-catch-finally 中，如果 catch 中 return 了，finally 还会执行吗？
会,finally是一定会执行的，会在return前执行。
#### 常见的异常类有哪些
- NullPointerException 空指针异常
- ClassNotFoundException 指定类不存在
- IndexOutOfBoundsException 数组下标越界异常
- FileNotFoundException 文件未找到异常
- IOException IO 异常
# 计算机网络
#### 1. 说说TCP三次握手的过程，为什么要用三次，两次不行吗？
用于保证可靠性和流控制机制的信息，包括Socket，序列号及窗口大小，TCP连接要在三种信息达成共识。

**第一次握手**：客户端向服务器发出连接请求报文，这时报文首部中的同部位SYN=1，同时随机生成初始序列号 seq=x，此时，TCP客户端进程进入了 SYN-SENT（同步已发送状态）状态。TCP规定，SYN报文段（SYN=1的报文段）不能携带数据，但需要消耗掉一个序号。这个三次握手中的开始。表示客户端想要和服务端建立连接。

**第二次握手**：TCP服务器收到请求报文后，如果同意连接，则发出确认报文。确认报文中应该 ACK=1，SYN=1，确认号是ack=x+1，同时也要为自己随机初始化一个序列号 seq=y，此时，TCP服务器进程进入了SYN-RCVD（同步收到）状态。这个报文也不能携带数据，但是同样要消耗一个序号。这个报文带有SYN(建立连接)和ACK(确认)标志，询问客户端是否准备好。

**第三次握手**：TCP客户进程收到确认后，还要向服务器给出确认。确认报文的ACK=1，ack=y+1，此时，TCP连接建立，客户端进入ESTABLISHED（已建立连接）状态。

TCP 连接使用三次握手的首要原因 —— 为了阻止历史的重复连接初始化造成的混乱问题，防止使用 TCP 协议通信的双方建立了错误的连接。

三次握手的原因
- 通过三次握手才能阻止重复历史连接初始化
- 通过三次握手才能对通信双方的初始序列号进行初始化
#### 2. http 响应码 301 和 302 代表的是什么？有什么区别？
- 301：永久移动。请求的资源已被永久的移动到新URI，返回信息会包括新的URI，浏览器会自动定向到新URI。今后任何新的请求都应使用新的URI代替
- 302：临时移动。资源只是临时被移动。客户端应继续使用原有URI

> URL是一种特殊的URI, 常用在网页上，用于网络上的资源定位、检索。
#### 3. 简述 tcp 和 udp的区别？
- TCP提供的是面向连接的、可靠的数据流传输，UDP提供的是非面向连接的、不可靠的数据流传输。
- TCP提供可靠的服务，通过TCP连接传送的数据，无差错、不丢失，不重复，按序到达；UDP尽最大努力交付，即不保证可靠交付。
- TCP面向字节流，UDP面向报文。


#### 4. OSI 的七层模型都有哪些？
物理层、链路层、网络层、传输层、会话层、表示层
#### 5. get 和 post 请求有哪些区别？
请求方法(Requst Method)表明对一个资源要进行什么操作。

GET和POST的http协议定义的两种请求方法
- GET 请求发送的数据信息显示在地址栏的URL中，POST请求则不会。
- GET 请求发送的数据有类型有限制，只能是具有ASCII码的数据，POST请求发送的数据没有类型限制，二进制类型也可以。
- GET 请求可被缓存,POST请求不能被缓存
- GET 请求保留在浏览器历史记录中，POST 请求不会保留在浏览器历史记录中
- GET 请求可被收藏为书签，POST 不能被收藏为书签
- GET 请求有长度限制，因为其发送数据的在URL中，POST 请求对数据长度没有要求，其发送的数据放在请求体中
- GET 请求只应当用于取回数据，不应在处理敏感数据时使用

#### 6. 什么是http协议
http 协议是**应用层**中的协议，用于网络浏览器和服务器之间的通信。Http是无状态的协议，即服务端不会保存请求端的数据(状态)
# 操作系统
#### 1. 进程的状态有哪些
分为三态模型和五态模型
- 五态模型
    - New/Created: 进程正在被创建，系统为其初始化PCB，分配资源。
    - Ready: 进程已获得除处理器外的所需资源，只是在等待分配处理器资源，只要分配了处理器进程就可执行。 
    - Running: 进程在被执行，占用CPU资源
    - Blocked/Waiting: 进程等待某种条件（如I/O操作或进程同步），在条件满足之前无法继续执行。该事件发生前即使把处理器资源分配给该进程，该进程也无法进行运行。
    - Exit: 进程正在从系统中撤销，回收进程的资源，撤销其PCB。

- 三态模型就是中间三个

#### 2. 进程的通信方式有哪些？
- 信号
- 信号量
- 管道
- 消息队列
- 共享内存
# java虚拟机
#### 1. 说一下 jvm 的主要组成部分？及其作用？
- 类加载器（ClassLoader）
- 运行时数据区（Runtime Data Area）
- 执行引擎（Execution Engine）
- 本地库接口（Native Interface）

组件的作用： 首先通过类加载器（ClassLoader）会把 Java 代码转换成字节码，运行时数据区（Runtime Data Area）再把字节码加载到内存中，而字节码文件只是 JVM 的一套指令集规范，并不能直接交个底层操作系统去执行，因此需要特定的命令解析器执行引擎（Execution Engine），将字节码翻译成底层系统指令，再交由 CPU 去执行，而这个过程中需要调用其他语言的本地库接口（Native Interface）来实现整个程序的功能

#### 2. 说一下 jvm 运行时数据区？
包含**5各部分**
- PC Register
- Java Virtual
- Heap
- Method Area
- Native Method Stack

PC Register: 记录java虚拟机当前执行的指令地址, 线程私有。 当前线程所执行的字节码的行号指示器，字节码解析器的工作是通过改变这个计数器的值，来选取下一条需要执行的字节码指令，分支、循环、跳转、异常处理、线程恢复等基础功能，都需要依赖这个计数器来完成

Java Virtual Mechine Stack: 存储Frame，线程独有。用于存储局部变量表、操作数栈、动态链接、方法出口等信息

Heap：存储对象，线程共享 要注意，这个“堆”并不是数据结构意义上的堆（Heap (data structure)，一种有序的树），而是动态内存分配意义上的堆——用于管理动态生命周期的内存区域。

Method Area: 存储每个类的结构，线程共享。 用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译后的代码等数据

Native Method Stack: 与虚拟机栈的作用是一样的，只不过虚拟机栈是服务 Java 方法的，而本地方法栈是为虚拟机调用 Native 方法服务的
#### 3. 说一下堆栈的区别？
- 功能方面：堆是用来存放对象的，栈是用来执行程序的
- 共享性：堆是线程共享的，栈是线程私有的
- 空间大小：堆大小远远大于栈
#### 3. 什么是双亲委派模型？
在介绍双亲委派模型之前先说下类加载器。对于任意一个类，都需要由加载它的类加载器和这个类本身一同确立在 JVM 中的唯一性，每一个类加载器，都有一个独立的类名称空间。类加载器就是根据指定全限定名称将 class 文件加载到 JVM 内存，然后再转化为 class 对象。

类加载器分类：
- 启动类加载器（Bootstrap ClassLoader），是虚拟机自身的一部分，用来加载Java_HOME/lib/目录中的，或者被 -Xbootclasspath 参数所指定的路径中并且被虚拟机识别的类库
- 其他类加载器：
- 扩展类加载器（Extension ClassLoader）：负责加载\lib\ext目录或Java. ext. dirs系统变量指定的路径中的所有类库
- 应用程序类加载器（Application ClassLoader）。负责加载用户类路径（classpath）上的指定类库，我们可以直接使用这个类加载器。一般情况，如果我们没有自定义类加载器默认就是用这个加载器

双亲委派模型：如果一个类加载器收到了类加载的请求，它首先不会自己去加载这个类，而是把这个请求委派给父类加载器去完成，每一层的类加载器都是如此，这样所有的加载请求都会被传送到顶层的启动类加载器中，只有当父加载无法完成加载请求（它的搜索范围中没找到所需的类）时，子加载器才会尝试去加载类

#### 4. 说一下类加载的执行过程？
类装载分为以下 5 个步骤：
1) 加载：根据查找路径找到相应的 class 文件然后导入
2) 检查：检查加载的 class 文件的正确性
3) 准备：给类中的静态变量分配内存空间
4) 解析：虚拟机将常量池中的符号引用替换成直接引用的过程。符号引用就理解为一个标示，而在直接引用直接指向内存中的地址
5) 初始化：对静态变量和静态代码块执行初始化工作

#### 5. 怎么判断对象是否可以被回收？
- **引用计数算法(Reference Counting)** 基本上没人用  
在对象中添加一个引用计数器，每当有一个地方引用它时，计数器值就加一；当引用失效时，计数器就减一。计数器为0的对象就不在被使用。
- **可达性分析算法**(Reachability Analysis)  
通过一系列“GC Roots”的根对象作为起始节点集，从这些节点开始，根据引用关系向下搜索，搜索过程中走过的路径称为引用链(Reference Chain)，如果某个对象到GC Roots间没有任何引用链相连，则证明此对象是不可能再被使用。
##### 什么对象可作为GC Root的对象
在Java中，可作为GC Root的对象包括以下几种：
- 虚拟机栈（栈帧中的本地变量表）中引用的对象
- 方法区中类静态属性引用的对象
- 方法区中常量引用的对象
- 本地方法栈中JNI（即一般说的Native方法）引用的对象

#### 6. java 中都有哪些引用类型？
- 强引用：发生 gc 的时候不会被回收
- 软引用：有用但不是必须的对象，在发生内存溢出之前会被回收
- 弱引用：有用但不是必须的对象，在下一次GC时会被回收
- 虚引用（幽灵引用/幻影引用）：无法通过虚引用获得对象，用 PhantomReference 实现虚引用，虚引用的用途是在 gc 时返回一个通知
#### 7. 说一下 jvm 有哪些垃圾回收算法？
1) **标记清除算法**  
首先标记出所有需要回收的对象，在标记完成后统一回收掉所有被标记的对象，也可以反过来。
2) **标记复制算法** 目前大多数jVM采用此算法回收新生代  
将可用的内存按容量划分为大小相等的两块，每次只使用其中的一块，当一块的内存用完了，就将还存活的对象复制到另外一块上面，然后再把已使用的那块内存空间一次清理掉。
3) **标记整理算法** 针对老年代提出的算法  
其标记过程与标记清除算法一致，但后续不是直接对对象进行清理，而是让所有存活的对象都向内存空间一端移动，然后直接清理掉边界以外的内存。

根据对象存活周期的不同将内存划分为几块，一般是新生代和老年代，新生代基本采用复制算法，老年代采用标记整理算法
#### 8. 说一下 jvm 有哪些垃圾回收器？
- Serial Garbage Collector
- Parallel Garbage Collector 
- CMS Garbage Collector 一种以获得最短停顿时间为目标的收集器，非常适用 B/S 系统
- G1 Garbage Collector 一种兼顾吞吐量和停顿时间的 GC 实现，是 JDK 9 以后的默认 GC 选项
#### 9. 详细介绍一下 CMS 垃圾回收器？
MS 是英文 Concurrent Mark-Sweep 的简称，是以牺牲吞吐量为代价来获得最短回收停顿时间的垃圾回收器。对于要求服务器响应速度的应用上，这种垃圾回收器非常适合。在启动 JVM 的参数加上“-XX:+UseConcMarkSweepGC”来指定使用 CMS 垃圾回收器

CMS 使用的是标记-清除的算法实现的，所以在 gc 的时候回产生大量的内存碎片，当剩余内存不能满足程序运行要求时，系统将会出现 Concurrent Mode Failure，临时 CMS 会采用 Serial Old 回收器进行垃圾清除，此时的性能将会被降低
#### 10. 新生代垃圾回收器和老生代垃圾回收器都有哪些？有什么区别？
- 新生代回收器：Serial、ParNew、Parallel Scavenge

- 老年代回收器：Serial Old、Parallel Old、CMS

- 整堆回收器：G1

新生代垃圾回收器一般采用的是复制算法，复制算法的优点是效率高，缺点是内存利用率低；老年代回收器一般采用的是标记-整理的算法进行垃圾回收
#### 11. 简述分代垃圾回收器是怎么工作的？
分代回收器有两个分区：老生代和新生代，新生代默认的空间占比总空间的 1/3，老生代的默认占比是 2/3

新生代使用的是复制算法，新生代里有 3 个分区：Eden、To Survivor、From Survivor，它们的默认占比是 8:1:1，它的执行流程如下：
- 把 Eden + From Survivor 存活的对象放入 To Survivor 区
- 清空 Eden 和 From Survivor 分区
- From Survivor 和 To Survivor 分区交换，From Survivor 变 To Survivor，To Survivor 变 From Survivor

每次在 From Survivor 到 To Survivor 移动时都存活的对象，年龄就 +1，当年龄到达 15（默认配置是 15）时，升级为老生代。大对象也会直接进入老生代。 老生代当空间占用到达某个值之后就会触发全局垃圾收回，一般使用标记整理的执行算法。以上这些循环往复就构成了整个分代垃圾回收的整体执行流程

#### 12. 说一下 jvm 调优的工具？
JDK 自带了很多监控工具，都位于 JDK 的 bin 目录下，其中最常用的是 jconsole 和 jvisualvm 这两款视图监控工具

- jconsole：用于对 JVM 中的内存、线程和类等进行监控；

- jvisualvm：JDK 自带的全能分析工具，可以分析：内存快照、线程快照、程序死锁、监控内存的变化、gc 变化等

#### 13. 常用的 jvm 调优的参数都有哪些？
- -Xss: 每个线程虚拟机栈的大小
- -Xms2g：初始化推大小为 2g

- -Xmx2g：堆最大内存为 2g

- -XX:NewRatio=4：设置年轻的和老年代的内存比例为 1:4

- -XX:SurvivorRatio=8：设置新生代 Eden 和 Survivor 比例为 8:2

#### 14. OOM说一下？怎么排查？哪些会导致OOM? OOM出现在什么时候

#### 15. 什么是Full GC？GC? major GC? stop the world

#### 16. 描述JVM中一次full gc过程。
## 代码
1) <a id="线程方法"> `wait`, `sleep`, `notify()`</a>
```java
// 两个线程交叉打印数字
class Number implements Runable{
	private int number = 1;
	@Override
	public void run(){
		while(true){
			synchronized(this){
				notify();
				if (number < 100) {
					try{
						Thread.sleep();
					} catch (InterruptedException e){
						e.printStackTrace();
					}
					System.out.println(Thread.currentThread().getName() + ":" + number);
					number++;
					try{
						wait();
					} catch (InterruptedException e){
						e.printStackTrace();
					}
				} else{
					break;
				}
			}
		}
	}
}
public class CommunicationTest{
	public static void main(String[] args) {
		Number number = new Number();
		Thread t1 = new Thread(number);
		Thread t2 = new Thread(number);
		t1.setName("线程1");
		t1.setName("线程2");
		t1.start();
		t1.start();
	}
}
```




