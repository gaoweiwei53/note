# 语法点
1) [`self =>`](https://stackoverflow.com/questions/52024763/what-is-the-self-meaning-in-new-class-of-scala)
# 2.13新特性
1) `2.13`最主要的更新是重新设计了标准库集合(Standard library collections)，使其更简洁、更安全、性能更好
2) 标准库：`Future`更快和更健壮
3) 语法方面：Literal types, partial unification, by-name implicits, more.
4) 编译器：更快了
# Scala Classes
## Constructor
- Primary constructor
```scala
class Person(var firstName: String, var lastName: String)
```
-  Auxiliary class constructors   
定义名叫 this的方法，遵循规则
    - 每一个auxiliary constructor必须要有不同的签名
    - 每一个constructor 必须调用其中一个前面已经定义的constructors
```scala
val DefaultCrustSize = 12
val DefaultCrustType = "THIN"

// the primary constructor
class Pizza (var crustSize: Int, var crustType: String) {

    // one-arg auxiliary constructor
    def this(crustSize: Int) = {
        this(crustSize, DefaultCrustType)
    }

    // one-arg auxiliary constructor
    def this(crustType: String) = {
        this(DefaultCrustSize, crustType)
    }

    // zero-arg auxiliary constructor
    def this() = {
        this(DefaultCrustSize, DefaultCrustType)
    }

    override def toString = s"A $crustSize inch pizza with a $crustType crust"

}
```
可以使用默认参数来代替上面的构造函数
```scala
class Socket(var timeout: Int = 2000, var linger: Int = 3000) {
    override def toString = s"timeout: $timeout, linger: $linger"
}
```
```scala
new Socket()
new Socket(1000)
new Socket(4000, 6000)
```
## Case Class
case class 和一般的类差不多，但是有一些自己的特性，默认会实现几个方法。在模式匹配中很好用。
使用case classes有很多好处，如下：
- Case class constructor parameters are public val fields by default, so accessor methods are generated for each parameter.
- An apply method is created in the companion object of the class, so you don’t need to use the new keyword to create a new instance of the class.
- An unapply method is generated, which lets you use case classes in more ways in match expressions。
- A copy method is generated in the class. You may not use this feature in Scala/OOP code, but it’s used all the time in Scala/FP.
- equals and hashCode methods are generated, which let you compare objects and easily use them as keys in maps.
- A default toString method is generated, which is helpful for debugging.
“the biggest advantage of case classes is that they support pattern matching.”
## Pattern match
**Pattern guards**:用在`case`中的布尔表达式：`if <boolean expression>`
## Sealed classes
Traits and classes可以被标识`sealed`, 表示其子类型必须在同一个文件中申明。主要为了模式匹配知道所有的情况。
## 单例类Singleton Objects
单例类是一种只能拥有一个对象的类，用`object`定义:
```scala
object Box
```
## Companion objects
和类名一样的object的叫Companion object，对应的这个类叫这个对象的companion class.他们可以互相访问彼此的私有成员
## Case objects
case object和普通的object类似，只是它具有更多的特性, 包括：
- It’s serializable
- It has a default `hashCode` implementation
- It has an improved `toString` implementation
代替普通的`object`主要用在两种地方：
- When creating enumerations
- When creating containers for “messages” that you want to pass between other objects (such as with the Akka actors library)
## trait
trait相当于java中的接口，在里面定义方法但是不用实现。PS: java8中接口里也可以有具体方法

实现多个接口用`with`:
```scala
trait Speaker {
    def speak(): String
}

trait TailWagger {
    def startTail(): Unit
    def stopTail(): Unit
}

trait Runner {
    def startRunning(): Unit
    def stopRunning(): Unit
}
class Dog extends Speaker with TailWagger with Runner {

    // Speaker
    def speak(): String = "Woof!"

    // TailWagger
    def startTail(): Unit = println("tail is wagging")
    def stopTail(): Unit = println("tail is stopped")

    // Runner
    def startRunning(): Unit = println("I'm running")
    def stopRunning(): Unit = println("Stopped running")

}
```
## 抽象类
抽象类和trait类似，一般用trait就行了，抽象类很少用。只在以下两种情况用：
1) 创建需要构造参数的基类
3) Scala代码会被java代码调用
```scala
abstract class Animal(name: String)
```
