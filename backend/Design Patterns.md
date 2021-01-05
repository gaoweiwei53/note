[资料1](https://www.runoob.com/design-pattern/design-pattern-tutorial.html)
[资料2](https://refactoringguru.cn/)
# Design Principles
## 1. 开闭原则 Open Close Principle(OCP)
对扩展开放，对修改关闭。在程序需要进行拓展的时候，要尽可能少地修改原有的代码，
## 2. 依赖反转原则二 Dependency Inversion Principle
- 高级模块不应该依赖低级模块，它们都应该依赖抽象
- 抽象不应该依赖具体，具体应该依赖抽象
## 3. 接口隔离原则 Interface Segregation Principle
原则：使用多个隔离的接口，比使用单个接口要好。不应该使用包含我们用不到的方法的“胖接口”
## 4. 单一职责原则 Single Responsibility Principle
一个类只负责一个功能
## 5. 里式替换原则 Liskov's Substitution Principle(LSP)
在设计程序模块的过程中，我们一直在创建一些类层次结构。然后我们扩展一些类，创建一些派生类。

我们必须确保新的派生类只是扩展，而不替换旧类的功能。否则，在现有程序模块中使用新类时，可能会产生不希望看到的效果。

Likov的替换原则指出，如果程序模块使用基类，则可以用派生类替换基类的引用，而不会影响程序模块的功能。

派生类型必须完全可替换其基类类型。这个原则只是开闭原则的扩展，它意味着我们必须确保新的派生类在不改变其行为的情况下扩展基类。

# Design Patterns
## 1.单例模式
所谓类的单例设计模式，就是采取一定的方法保证在整个的软件系统中，对某个类只能存在一个对象实例， 并且该类只提供一个取得其对象实例的方法(静态方法)。

**实现：** 我们将创建一个 SingleObject 类。SingleObject 类有它的私有构造函数和本身的一个静态实例。

注意事项：getInstance() 方法中需要使用同步锁 synchronized (Singleton.class) 防止多线程同时进入造成 instance 被多次实例化。
```java
public class SingleObject {
 
   //创建 SingleObject 的一个对象
   private static SingleObject instance = new SingleObject();
 
   //让构造函数为 private，这样该类就不会被实例化
   private SingleObject(){}
 
   //获取唯一可用的对象
   public static SingleObject getInstance(){
      return instance;
   }
 
   public void showMessage(){
      System.out.println("Hello World!");
   }
}
```
## 2. 工厂方法模式(Factory Method)
定义一个用于创建对象的接口，让子类决定实例化哪一个类，使一个类的实例化延迟到其子类

工厂方法是一种创建设计模式，它提供了在超类中创建对象的接口，但允许子类更改将要创建的对象的类型。

它的核心结构有四个角色：抽象工厂/Creator，具体工厂/Concrete Creator，抽象产品，具体产品
## 3. 抽象工厂模式 (Abstract Factory)
提供一个创建一系列相关或相互依赖对象的接口，而无需指定他们具体的类
它的核心结构也是四个角色：抽象工厂/Creator，具体工厂/Concrete Creator，抽象产品，具体产品
> 工厂方法模式只有一个抽象产品类，而抽象工厂模式有多个
工厂方法模式的具体工厂类只能创建一个具体产品类的实例，而抽象工厂模式可以创建多个。
当产品只有一个的时候，抽象工厂模式变成工厂模式。当工厂模式的产品变为多个时，工厂模式变为抽象工厂模式
