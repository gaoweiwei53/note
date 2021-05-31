# 常用JVM参数
## 内存
[参考](https://youzhixueyuan.com/jvm-memory-model-and-parameter-configuration.html)
1) `-Xms` 表示JVM初始化时的堆内存大小，单位可以是k, m, g
2) `-Xmx` 表示JVM可使用最大的堆内存。
> 最好设置相同大小

## GC
JVM has four types of GC implementations:
- **Serial** Garbage Collector
- **Parallel** Garbage Collector
- **CMS** Garbage Collector
- **G1** Garbage Collector

可以使用以下参数设置：
```shell
-XX:+UseSerialGC
-XX:+UseParallelGC
-XX:+USeParNewGC
-XX:+UseG1GC
```
# 名词解释
**字面量(Literal)**: a literal is a notation for representing a fixed value in source code.字面量是代码中固定值的表示，`x = 1;`中 `x`是变量(Variable), `1`是字面量。  
**句柄(handle)**: a handle is an abstract reference to a resource.Common resource handles include file descriptors, network sockets, database connections, process identifiers (PIDs), and job IDs.
# 虚拟机类加载机制
## 运行时数据区域
[参考1](https://blog.csdn.net/xiaojin21cen/article/details/104267301)  
[参考2](https://www.oracle.com/webfolder/technetwork/tutorials/mooc/JVM_Troubleshooting/week1/lesson1.pdf)  
[参考3](https://docs.oracle.com/javase/specs/jvms/se11/html/index.html)  
![运行时数据区](https://upload-images.jianshu.io/upload_images/14923529-c0cbbccaa6858ca1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
![运行时数据区](../resources/JVM-Architecture.png)

### 1) 程序计数器(PC)
可以看作当前线程所执行的字节码的行号指示器，字节码解释器工作时就是通过改变这个计数器的值来选取下一条需要执行的字节码指令
### 2) JVM Stack
每个方法被执行的时候，Java虚拟机都会同步创建一个栈帧(Stack Frame).

每一个frame都有它自己的局部变量数组(array of local varibles)、操作数栈(operand stack)、指向当前方法的所属的类的运行时常量池的引用。
> 所以java中的局部变量时在栈上分配的！

frame也可以被扩展存储一些其他信息，如debugging信息

一个特定的时刻，只有一个当前正在执行的方法的frame时活跃的(active)，这个frame被称为current frame，它的方法被称为current method, 定义这个方法的类被称为current class
#### 局部变量
一个局部变量可以存储boolean, byte, char, short, int, float, reference, or returnAddress. 调用方法时，在jvm使用使用局部变量来传递参数。当一个方法调用时，总使用0号局部变量来传递对调用实例方法的对象的引用(this)
#### 操作数栈
Frame包含一个LIFO的栈，叫做操作数栈。
#### 动态链接？？？
### 3) 本地方法栈(Native Method Stack)
虚拟机栈为虚拟机执行Java方法服务，而本地方法栈则是为虚拟机使用到的本地方法服务。
### 4) 堆 Heap
存放对象实例，是垃圾收集器管理的内存区域。

堆内存包括：
- 新生代
    - Eden
    - Survivor
        - from
        - to
- 老年代
### 5) 方法区 (Method Area)
用于存储已被虚拟机加载的类型信息、常量、静态变量、即时编译器编译后的代码缓存等数据。

在Java7中永久代中存储的部分数据已经开始转移到Java Heap或Native Memory中了。比如字符串常量池(interned strings)和类的静态变量(class statics)转移到了Java Heap。[参考](http://openjdk.java.net/jeps/122)

永久代的内存地址与堆是连续的，而元空间不是的。用元空间取代永久代的原因是解决永久代容易发送内存溢出的原因。元空间使用的是本地内存。
##### 类的信息
- 类的完整名称（比如，java.long.String）
- 类的直接父类的完整名称
- 类的直接实现接口的有序列表（因为一个类直接实现的接口可能不止一个，因此放到一个有序表中）
- 类的修饰符

##### 运行时常量池
每一个Class文件中，都维护着一个常量池（这个保存在类文件里面，不要与方法区的运行时常量池搞混），里面存放着编译时期生成的各种字面值和符号引用；这个常量池的内容，在类加载的时候，被复制到方法区的运行时常量池
##### 字段信息
- 声明的顺序
- 修饰符
- 类型
- 名字
##### 方法信息
- 声明的顺序
- 修饰符
- 返回值类型
- 名字
- 参数列表（有序保存）
- 异常表（方法抛出的异常）
- 方法字节码（native、abstract方法除外，）
- 操作数栈和局部变量表大小
##### 对类加载器的引用
jvm必须知道一个类型是由启动加载器加载的还是由用户类加载器加载的。如果一个类型是由用户类加载器加载的，那么jvm会将这个类加载器的一个引用作为类型信息的一部分保存在方法区中。

##### 对Class类的引用
jvm为每个加载的类都创建一个java.lang.Class的实例（存储在堆上）。而jvm必须以某种方式把Class的这个实例和存储在方法区中的类型数据（类的元数据）联系起来， 因此，类的元数据里面保存了一个Class对象的引用；


##### 方法表
#### 运行时常量池 (Runtime Constant Pool)
运行时常量池是方法区的一部分。Class文件除了有类的版本、字段、方法、接口等描述信息外，还有一项信息常量池表(Constant Pool Table)，用于存放编译期生成的各种字面量与符号引用，这部分内容将在类加载后存放到方法区的运行时常量池中。

运行时常量池时每个类或接口在class文件里的常量池表(Constant Pool Table)的运行时表示。它包含各种常量，如在编译期已知的数字字面量和必须在运行时解析的字段引用。
## 类的生命周期

### 类的加载过程
加载 --> 验证 --> 准备 --> 解析 --> 初始化
### 加载
在加载阶段虚拟机需要完成以下三件事：
1) 通过一个类的全限定名来获取定义此类的二进制字节流。
2) 将这个字节流所代表的静态存储结构转化为**方法区**的运行时数据结构。
3) 在内存中生成一个代表这个类的java.lang.Class对象，作为方法区这个类的各种数据的访问入口。
### 验证
验证是Linking阶段的第一步，这一步的目的是确保Class文件的字节流中包含的信息符合《Java虚拟机规范》的全部约束要求，保证这些信息被当作代码运行后不会危害虚拟机的自身安全。
验证阶段大致会完成四个检验：
- 文件格式验证：验证字节流是否符合Class文件格式规范。
- 元数据验证：对类的元数据信息进行语义校验，保证不存在与《Java虚拟机规范》定义相悖的元数据信息。
- 字节码验证：
- 符号引用验证
## 准备
正式为类中定义的变量（即静态变量，被static修饰的变量）分配内存并设置变量初始值。从概念上讲，这些变量所使用的内存都应当在方法区中进行分配。
## 解析
解析阶段是Java虚拟机将常量池内的符号引用替换为直接引用的过程。
- 符号引用(Symbolic References): 符号引用以一组符号来描述所引用的目标，符号可以是任何形式的字面量，只要使用时能无歧义地定位到目标即可。
- 直接引用(Direc Refernces): 直接引用是可以直接指向目标的指针、相对偏移量或者是一个能间接定位到目标的句柄。

解析动作主要针对类或接口、字段、类方法、接口方法、方法类型、方法句柄和调用点 限定符这7类符合引用进行。
## 初始化
根据程序员通过程序编码制定的主观计划去初始化类变量和其他资源。也就是执行类构造器`<Clinit>()`方法的过程
## 类加载器
对于任意一个类，都必须由加载它的类加载器和这个类本身一起共同确立其在Java虚拟机中的唯一性。
三层类加载器：启动类加载器 <-- 扩展类加载器 <-- 应用类加载器
各种类加载器之间的层次关系被称为类加载器的“双亲委派模型”
双亲委派模型的工作过程是：如果一个类加载器收到了类加载的请求，它首先不会自己去尝试加载这个类，而是把这个请求委派给父加载器去完成，每一个层次的类加载器都是如此，只有当父加载器反馈自己无法完成这个加载请求时，子类加载器才尝试自己去完成。
## [Java模块化系统](https://www.oracle.com/corporate/features/understanding-java-9-modules.html)
JDK9 引入了Java模块化系统，目标：可配置的封装隔离机制。
模块就是一组jar包的集合，同时在module-info.java里定义以下内容：
- `requires` 需要依赖哪个模块
There is also a requires static directive to indicate that a module is required at compile time, but is optional at runtime.   
requires transitive—implied readability. To specify a dependency on another module and to ensure that other modules reading your module also read that dependency.
- `exports`/`exports…to`: 声明该模块中的 `public` 类型的**包**可以被哪些其他模块所使用。
- `provides…with`：声明模块提供的service
- `uses`：声明模块使用的service，services是类的实例，其实现了接口或扩展了抽象类。
- `opens`：其他模块可反射访问模块的列表 
### 模块下的类加载器
扩展类加载器被平台类加载器取代。当平台及应用程序类加载器收到类加载请求，在委派给父加载器前，要判断该类是否能够归属到某一系统模块中，如果找到，就要优先委派给负责那个模块的加载器完成加载。

启动类加载器 <-- 平台类加载器 <--> 应用类加载器
# 垃圾收集器与内存分配策略
GC对堆进行回收之前，首先确定哪些对象已死。有以下算法：
1) **引用计数算法**(Reference Counting) 基本上没人用  
在对象中添加一个引用计数器，每当有一个地方引用它时，计数器值就加一；当引用失效时，计数器就减一。计数器为0的对象就不在被使用。
2) **可达性分析算法**(Reachability Analysis)  
通过一系列“GC Roots”的根对象作为起始节点集，从这些节点开始，根据引用关系向下搜索，搜索过程中走过的路径称为引用链(Reference Chain)，如果某个对象到GC Roots间没有任何引用链相连，则证明此对象是不可能再被使用。
- 强引用
- 软引用
- 弱引用
- 虚引用
## 垃圾收集算法
- 新生代
- 老年代
1) **标记清除算法**  
首先标记出所有需要回收的对象，在标记完成后统一回收掉所有被标记的对象，也可以反过来。
2) **标记复制算法** 目前大多数jVM采用此算法回收新生代  
将可用的内存按容量划分为大小相等的两块，每次只使用其中的一块，当一块的内存用完了，就将还存活的对象复制到另外一块上面，然后再把已使用的那块内存空间一次清理掉。
3) **标记整理算法** 针对老年代提出的算法  
其标记过程与标记清除算法一致，但后续不是直接对对象进行清理，而是让所有存活的对象都向内存空间一端移动，然后直接清理掉边界以外的内存。
