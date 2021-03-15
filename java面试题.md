# Java
## 1. Java基本数据类型几个？
Java提供了八种基本类型。六种数字类型（四个整数型，两个浮点型），一种字符类型，还有一种布尔型。
- byte   1字节
- short  2字节
- int    4字节
- long   8字节
- float  4字节
- double 8字节
- char   2字节
- boolen 表示1位的信息
## 2. StringBuffer和StringBuilder的区别？
和String类不同的是，StringBuffer 和 StringBuilder 类的对象能够被多次的修改，并且不产生新的未使用对象。
- StringBuffer 和 StringBuilder 长度可变

- StringBuffer 线程安全 StringBuilder 线程不安全

- StringBuilder 速度快，推荐使用
## JDK 和 JRE 有什么区别？
JRE 是 java运行时(java runtime environment), 包含了java虚拟机，java基础类库，是java程序运行所必须的环境。

JDK是java开发工具，是开发java应用的工具包，其包括jre，还有其他开发java应用的工具，例如javac编译器
## equals 和 ==的区别？
- equals()比较两个对象的内容
- 对于基本类型，比较的是值，对于对象，比较的是对象在内存中的地址。
##  Hashcode
- hashCode()相等的两个对象他们的equal()不一定相等。
- equal()相等的两个对象他们的hashCode()一定相等。
## final修饰符的作用
 final修饰符用来修饰类、方法和变量，final 修饰的类不能够被继承，修饰的方法不能被继承类重写，修饰的变量为常量，是不可修改的。
## Math.round(-1.5)的值
等于 -1, round()返回四舍五入，负.5小数返回较大整数，如-1.5返回-1，1.5返回2
## String str="i"与 String str=new String("i")一样吗？
不一样，因为内存的分配方式不一样。String str = "i"的方式，Java虚拟机会将其分配到常量池中，而常量池中没有重复的元素；String str = new String("i")则会被分到堆内存中。
