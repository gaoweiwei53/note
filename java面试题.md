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
2) `Executors`工厂
    ```java
    ExecutorService service = Executors.newFixedThreadPool(8);
    ExecutorService service = Executors.newCachedThreadPool(8);
    ExecutorService service = Executors.newScheduledThreadPool(8);
    ExecutorService service = Executors.newFixedThreadPool(8);
    ```
# 计算机网络
#### 1. 说说TCP三次握手的过程，为什么要用三次，两次不行吗？
# 操作系统
#### 1. 进程的状态有哪些
分为三态模型和五态模型
- 三态模型
- 五态模型



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




