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
Block IO，客户端有连接请求时服务器端就需要启动一个线程进行处理，如果这个连接不做任何事情会造成不必要的线程开销，当然可以通过线程池机制改善。
- Java NIO  
No block IO，客户端发送的连接请求都会注册到多路复用器上，多路复用器轮询到连接有I/O请求时才启动一个线程进行处理。
- Java AIO(NIO.2)  
Asynchronous non-blocking IO, 客户端的I/O请求都是由OS先完成了再通知服务器应用去启动线程进行处理

#### 13. 说说对面向对象的理解？
#### 14. 说说java8 java9 java11的新特性？函数式接口
#### 15. `instanceof`的作用？
a instanceof A: 判断a是否位类A的实例
#### 16. 说说红黑树？
二叉搜索树 --> 平衡树 --> 

（1）每个节点或者是黑色，或者是红色。
（2）根节点是黑色。
（3）每个叶子节点（NIL）是黑色。 [注意：这里叶子节点，是指为空(NIL或NULL)的叶子节点！]
（4）如果一个节点是红色的，则它的子节点必须是黑色的。
（5）从一个节点到该节点的子孙节点的所有路径上包含相同数目的黑节点。
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
#### 17. 说一下 synchronized 底层实现原理？
#### 18. synchronized 和 Lock 有什么区别？
#### 深拷贝和浅拷贝区别是什么？
拷贝分为引用拷贝和对象拷贝，深拷贝和浅拷贝都属于对象拷贝。不同的是如果拷贝对象里有字段引用了其他对象，浅拷贝只会拷贝该字段的引用，而深拷贝会将该字段引用的对象也进行拷贝。

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
- GET 请求可被缓存
- GET 请求保留在浏览器历史记录中
- GET 请求可被收藏为书签
- GET 请求不应在处理敏感数据时使用
- GET 请求有长度限制
- GET 请求只应当用于取回数据

- POST 请求不会被缓存
- POST 请求不会保留在浏览器历史记录中
- POST 不能被收藏为书签
- POST 请求对数据长度没有要求
# 操作系统
#### 1. 进程的状态有哪些
分为三态模型和五态模型
- 三态模型
- 五态模型
# java虚拟机
#### 1. 说一下 jvm 的主要组成部分？及其作用？

#### 说一下 jvm 运行时数据区？

#### 说一下堆栈的区别？

#### 队列和栈是什么？有什么区别？

#### 什么是双亲委派模型？

#### 说一下类加载的执行过程？

#### 怎么判断对象是否可以被回收？

#### java 中都有哪些引用类型？

#### 说一下 jvm 有哪些垃圾回收算法？

#### 说一下 jvm 有哪些垃圾回收器？

#### 详细介绍一下 CMS 垃圾回收器？

#### 新生代垃圾回收器和老生代垃圾回收器都有哪些？有什么区别？

#### 简述分代垃圾回收器是怎么工作的？

#### 说一下 jvm 调优的工具？

#### 常用的 jvm 调优的参数都有哪些？
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




