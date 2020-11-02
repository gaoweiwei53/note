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
# Case Classes
case class 和一般的类差不多，但是有一些自己的特性，默认会实现几个方法。在模式匹配中很好用。
使用case classes有很多好处，如下：
- Case class constructor parameters are public val fields by default, so accessor methods are generated for each parameter.
- An apply method is created in the companion object of the class, so you don’t need to use the new keyword to create a new instance of the class.
- An unapply method is generated, which lets you use case classes in more ways in match expressions。
- A copy method is generated in the class. You may not use this feature in Scala/OOP code, but it’s used all the time in Scala/FP.
- equals and hashCode methods are generated, which let you compare objects and easily use them as keys in maps.
- A default toString method is generated, which is helpful for debugging.
“the biggest advantage of case classes is that they support pattern matching.”
# Pattern match
**Pattern guards**:用在`case`中的布尔表达式：`if <boolean expression>`
## Sealed classes
Traits and classes可以被标识`sealed`, 表示其子类型必须在同一个文件中申明。主要为了模式匹配知道所有的情况。
## Singleton Objects
An object is a value. The definition of an object looks like a class, but uses the keyword `object`:
```scala
object Box
```
### Companion objects
和类名一样的object的叫Companion object，对应的这个类叫这个对象的companion class.他们可以互相访问彼此的私有成员
## Case objects
A case object is like an object, a case object has more features than a regular object. 包括：
- It’s serializable
- It has a default `hashCode` implementation
- It has an improved `toString` implementation
主要代替普通的`object`用在两种地方：
- When creating enumerations
- When creating containers for “messages” that you want to pass between other objects (such as with the Akka actors library)
