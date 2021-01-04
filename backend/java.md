A variable provides us with named storage that our programs can manipulate. Java provides three types of variables.

 -   **Class variables**  − Class variables also known as static variables are declared with the static keyword in a class, but outside a method, constructor or a block. There would only be one copy of each class variable per class, regardless of how many objects are created from it.
    
 -   **Instance variables**  − Instance variables are declared in a class, but outside a method. When space is allocated for an object in the heap, a slot for each instance variable value is created. Instance variables hold values that must be referenced by more than one method, constructor or block, or essential parts of an object's state that must be present throughout the class.
    
 -   **Local variables**  − Local variables are declared in methods, constructors, or blocks. Local variables are created when the method, constructor or block is entered and the variable will be destroyed once it exits the method, constructor, or block.
 ## **[Java：String、StringBuffer 和 StringBuilder 的区别](https://www.runoob.com/w3cnote/java-different-of-string-stringbuffer-stringbuilder.html)**
 String：字符串常量，字符串长度不可变。Java中String 是immutable（不可变）的。用于存放字符的数组被声明为final的，因此只能赋值一次，不可再更改。

StringBuffer：字符串变量（Synchronized，即线程安全）。如果要频繁对字符串内容进行修改，出于效率考虑最好使用 StringBuffer，如果想转成 String 类型，可以调用 StringBuffer 的 toString() 方法。Java.lang.StringBuffer 线程安全的可变字符序列。在任意时间点上它都包含某种特定的字符序列，但通过某些方法调用可以改变该序列的长度和内容。可将字符串缓冲区安全地用于多个线程。

StringBuilder：字符串变量（非线程安全）。在内部 StringBuilder 对象被当作是一个包含字符序列的变长数组。

**基本原则：**

-   如果要操作少量的数据用 String ；
-   单线程操作大量数据用StringBuilder ；
-   多线程操作大量数据，用StringBuffer。
- 最简单的回答是，stringbuffer 基本没有适用场景，你应该在所有的情况下选择使用 stringbuiler
# 不是很懂
 - [initial](https://docs.oracle.com/javase/tutorial/java/javaOO/initial.htmlhttps://docs.oracle.com/javase/tutorial/java/javaOO/initial.html)
 - [lambda expression](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html)
 - [ Generic Methods and Bounded Type Parameters](https://docs.oracle.com/javase/tutorial/java/generics/boundedTypeParams.html)
 - [ Integrating Default Methods into Existing Libraries](https://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)
 - [Effects of Type Erasure and Bridge Methods](https://docs.oracle.com/javase/tutorial/java/generics/bridgeMethods.html)
 - [Non-Reifiable Types](https://docs.oracle.com/javase/tutorial/java/generics/nonReifiableVarargsType.html)
 - [Reflection](https://docs.oracle.com/javase/tutorial/reflect/index.html)
# 重要
 - [genTypeInference](https://docs.oracle.com/javase/tutorial/java/generics/genTypeInference.html)
 - [Collection](https://docs.oracle.com/javase/tutorial/collections/index.html)


# Tricks

 - `'n' - '0'` 可将 数字的字符类型转化为 *Integer* 类型
 - `n + ""` 可将 其他类型 转化为 *String* 类型
 - 获取变量类型的方法
```
    public static String getType(Object o){ 
    	return o.getClass().toString();  
    }
```
# Collection
 - `List<String> list = new ArrayList<>()` 为什么要写成这样？
为了解耦 (decouple)，当后面代码使用到了`list`对象，若要修改`list`的实现类，直接修改这里就行了。
 - In JDK 8 and later, the `Collection` interface also exposes methods `Stream<E> stream()` and `Stream<E> parallelStream()`, for obtaining sequential or parallel streams from the underlying collection. (See the lesson entitled [Aggregate Operations](https://docs.oracle.com/javase/tutorial/collections/streams/index.html) for more information about using streams.)
 ## Traversing Collections
There are three ways to traverse collections: (1) using aggregate operations (2) with the  `for-each`  construct and (3) by using  `Iterator`s.
### Aggregate Operations

In JDK 8 and later, the preferred method of iterating over a collection is to obtain a stream and perform aggregate operations on it. Aggregate operations are often used in conjunction with lambda expressions to make programming more expressive, using less lines of code. The following code sequentially iterates through a collection of shapes and prints out the red objects:

    myShapesCollection.stream()
    .filter(e -> e.getColor() == Color.RED)
    .forEach(e -> System.out.println(e.getName()));
  Use  `Iterator`  instead of the  `for-each`  construct when you need to:

-   Remove the current element. The  `for-each`  construct hides the iterator, so you cannot call  `remove`. Therefore, the  `for-each`  construct is not usable for filtering.
-   Iterate over multiple collections in parallel.
## Collection Interface Bulk Operations
_Bulk operations_  perform an operation on an entire  `Collection`. You could implement these shorthand operations using the basic operations, though in most cases such implementations would be less efficient. The following are the bulk operations:

-   `containsAll`  — returns  `true`  if the target  `Collection`  contains all of the elements in the specified  `Collection`.
-   `addAll`  — adds all of the elements in the specified  `Collection`  to the target  `Collection`.
-   `removeAll`  — removes from the target  `Collection`  all of its elements that are also contained in the specified  `Collection`.
-   `retainAll`  — removes from the target  `Collection`  all its elements that are  _not_  also contained in the specified  `Collection`. That is, it retains only those elements in the target  `Collection`  that are also contained in the specified  `Collection`.
-   `clear`  — removes all elements from the  `Collection`.
## The Set Interface
A  [`Set`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html)  is a  [`Collection`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)  that cannot contain duplicate elements. It models the mathematical set abstraction. The  `Set`  interface contains  _only_  methods inherited from  `Collection`  and adds the restriction that duplicate elements are prohibited.  `Set`  also adds a stronger contract on the behavior of the  `equals`  and  `hashCode`  operations, allowing  `Set`  instances to be compared meaningfully even if their implementation types differ. Two  `Set`  instances are equal if they contain the same elements.

The Java platform contains three general-purpose  `Set`  implementations:  `HashSet`,  `TreeSet`, and  `LinkedHashSet`.  [`HashSet`](https://docs.oracle.com/javase/8/docs/api/java/util/HashSet.html), which stores its elements in a hash table, is the best-performing implementation; however it makes no guarantees concerning the order of iteration.  [`TreeSet`](https://docs.oracle.com/javase/8/docs/api/java/util/TreeSet.html), which stores its elements in a red-black tree, orders its elements based on their values; it is substantially slower than  `HashSet`.  [`LinkedHashSet`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedHashSet.html), which is implemented as a hash table with a linked list running through it, orders its elements based on the order in which they were inserted into the set (insertion-order).  `LinkedHashSet`  spares its clients from the unspecified, generally chaotic ordering provided by  `HashSet`  at a cost that is only slightly higher.
## The List Interface

A  [`List`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)  is an ordered  [`Collection`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)  (sometimes called a  _sequence_). Lists may contain duplicate elements.
The Java platform contains two general-purpose `List` implementations. [`ArrayList`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html), which is usually the better-performing implementation, and [`LinkedList`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) which offers better performance under certain circumstances.

## The Queue Interface
A [`Queue`](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html) is a collection for holding elements prior to processing. Besides basic `Collection` operations, queues provide additional insertion, removal, and inspection operations.
## The Deque Interface
Usually pronounced as `deck`, a deque is a double-ended-queue. A double-ended-queue is a linear collection of elements that supports the insertion and removal of elements at both end points. The `Deque` interface is a richer abstract data type than both `Stack` and `Queue` because it implements both stacks and queues at the same time. The [`Deque`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html) interface, defines methods to access the elements at both ends of the `Deque` instance. Methods are provided to insert, remove, and examine the elements. Predefined classes like [`ArrayDeque`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayDeque.html) and [`LinkedList`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) implement the `Deque` interface.
## The Map Interface

A  [`Map`](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html)  is an object that maps keys to values. A map cannot contain duplicate keys: Each key can map to at most one value. It models the mathematical  _function_  abstraction. The  `Map`  interface includes methods for basic operations (such as  `put`,  `get`,  `remove`,  `containsKey`,  `containsValue`,  `size`, and  `empty`), bulk operations (such as  `putAll`  and  `clear`), and collection views (such as  `keySet`,  `entrySet`, and  `values`).

The Java platform contains three general-purpose  `Map`  implementations:  [`HashMap`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html),  [`TreeMap`](https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html), and  [`LinkedHashMap`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedHashMap.html). Their behavior and performance are precisely analogous to  `HashSet`,  `TreeSet`, and  `LinkedHashSet`,
# Classes and Object
## covariant return type
When a method uses a class name as its return type, the class of the type of the returned object must be either a subclass of, or the exact class of, the return type
## Static
static 修饰符，用来修饰类方法和类变量
final 修饰符，用来修饰类、方法和变量，final 修饰的类不能够被继承，修饰的方法不能被继承类重新定义，修饰的变量为常量，是不可修改的。
abstract 修饰符，用来创建抽象类和抽象方法。
synchronized 和 volatile 修饰符，主要用于线程的编程。
-   **静态变量：**
    
    static 关键字用来声明独立于对象的静态变量，无论一个类实例化多少对象，它的静态变量只有一份拷贝。 静态变量也被称为类变量。局部变量不能被声明为 static 变量。
    
-   **静态方法：**
    
    static 关键字用来声明独立于对象的静态方法。静态方法不能使用类的非静态变量。静态方法从参数列表得到数据，然后计算这些数据。
 
 不用创建类的对象，直接使用类名调用static方法。
### synchronized 修饰符
synchronized 关键字声明的方法同一时间只能被一个线程访问。
### transient 修饰符
序列化的对象包含被 transient 修饰的实例变量时，java 虚拟机(JVM)跳过该特定的变量。
### volatile 修饰符
volatile 修饰的成员变量在每次被线程访问时，都强制从共享内存中重新读取该成员变量的值。而且，当成员变量发生变化时，会强制线程将变化值回写到共享内存。这样在任何时刻，两个不同的线程总是看到某个成员变量的同一个值。

## Using  `this`  with a Constructor
From within a constructor, you can also use the `this` keyword to call another constructor in the same class. Doing so is called an _**explicit constructor invocation**_.

    public class Rectangle {
        private int x, y;
        private int width, height;
        public Rectangle() {
            **this(0, 0, 1, 1);**
        }
        public Rectangle(int width, int height) {
            **this(0, 0, width, height);**
        }
        public Rectangle(int x, int y, int width, int height) {
            this.x = x;
            this.y = y;
            this.width = width;
            this.height = height;
        }
        ...
    }
If present, the invocation of another constructor must be the first line in the constructor.
对this的调用必须是构造器中的第一个语句
## Static method
A common use for static methods is to access static fields. For example, we could add a static method to the  `Bicycle`  class to access the  `numberOfBicycles`  static field:

    public static int getNumberOfBicycles() {
        return numberOfBicycles;
    }
如果不用**static**，则需要new个新对象才能调用这个方法
## Constants
The `static` modifier, in combination with the `final` modifier, is also used to define constants. The `final` modifier indicates that the value of this field cannot change.
## Nested Classes\
 
**Terminology:** Nested classes are divided into two categories: static and non-static. Nested classes that are declared `static` are called _static nested classes_. Non-static nested classes are called _**inner classes**_

        class OuterClass {
        ...
        static class StaticNestedClass {
            ...
        }
        class InnerClass {
            ...
        }
    }
A nested class is a member of its enclosing class. Non-static nested classes (**inner classes**) *have access* to other members of the enclosing class, even if they are declared private. Static nested classes do not have access to other members of the enclosing class. As a member of the `OuterClass`, a nested class can be declared `private`, `public`, `protected`, or _package private_. (Recall that outer classes can only be declared `public` or _package private_.)
### Why Use Nested Classes?
* **It is a way of logically grouping classes that are only used in one place**
* **It increases encapsulation**
* **It can lead to more readable and maintainable code**
## Static Nested Classes
like static class methods, a static nested class **cannot refer directly to** instance variables or methods defined in its enclosing class: it can use them only through an object reference.
**An static inner class does not have access to the instance fields of the outer class**
Static nested classes are accessed using the enclosing class name:

    OuterClass.StaticNestedClass
## Inner Classes
an inner class is associated with an instance of its enclosing class and **has direct access to** that object's methods and fields.Also, because an inner class is associated with an instance, **it cannot define any static** members itself.
To instantiate an inner class, you must first instantiate the outer class. Then, create the inner object within the outer object with this syntax:

    OuterClass.InnerClass innerObject = outerObject.new InnerClass();
    
    public class ShadowTest {

    public int x = 0;

    class FirstLevel {

        public int x = 1;

        void methodInFirstLevel(int x) {
            System.out.println("x = " + x);
            System.out.println("this.x = " + this.x);
            System.out.println("ShadowTest.this.x = " + ShadowTest.this.x);
        }
    }

    public static void main(String... args) {
        ShadowTest st = new ShadowTest();
        ShadowTest.FirstLevel fl = st.new FirstLevel();
        fl.methodInFirstLevel(23);
    }
}

The following is the output of this example:

x = 23
this.x = 1
ShadowTest.this.x = 0
There are two special kinds of inner classes: [local classes](https://docs.oracle.com/javase/tutorial/java/javaOO/localclasses.html) and [anonymous classes](https://docs.oracle.com/javase/tutorial/java/javaOO/anonymousclasses.html).

## Serialization

[Serialization](https://docs.oracle.com/javase/tutorial/jndi/objects/serial.html) of inner classes, including [local](https://docs.oracle.com/javase/tutorial/java/javaOO/localclasses.html) and [anonymous](https://docs.oracle.com/javase/tutorial/java/javaOO/anonymousclasses.html) classes, is strongly discouraged.
When the Java compiler compiles certain constructs, such as inner classes, it creates _synthetic constructs_; these are classes, methods, fields, and other constructs that do not have a corresponding construct in the source code. Synthetic constructs enable Java compilers to implement new Java language features without changes to the JVM. However, synthetic constructs can vary among different Java compiler implementations, which means that `.class` files can vary among different implementations as well. Consequently, you may have compatibility issues if you serialize an inner class and then deserialize it with a different JRE implementation. See the section [Implicit and Synthetic Parameters](https://docs.oracle.com/javase/tutorial/reflect/member/methodparameterreflection.html#implcit_and_synthetic) in the section [Obtaining Names of Method Parameters](https://docs.oracle.com/javase/tutorial/reflect/member/methodparameterreflection.html) for more information about the synthetic constructs generated when an inner class is compiled.
# Local Classes
Local classes are classes that are defined in a _block_, which is a group of zero or more statements between balanced braces. You typically find local classes defined in the body of a method.
You can define a local class inside any block. For example, you can define a local class in a method body, a `for` loop, or an `if` clause.
In addition, a local class has access to local variables. However, a local class can only access local variables that are declared **final**.
Starting in Java SE 8, if you declare the local class in a method, it can access the method's parameters. For example, you can define the following method in the  `PhoneNumber`  local class:

    public void printOriginalNumbers() {
        System.out.println("Original numbers are " + phoneNumber1 +
            " and " + phoneNumber2);
    }

The method  `printOriginalNumbers`  accesses the parameters  `phoneNumber1`  and  `phoneNumber2`  of the method  `validatePhoneNumber`.
## Local Classes Are Similar To Inner Classes
Local classes are similar to inner classes because they cannot define or declare any static members. Local classes in static methods, such as the class `PhoneNumber`, which is defined in the static method `validatePhoneNumber`, can only refer to static members of the enclosing class.
Local classes are non-static because they have access to instance members of the enclosing block. Consequently, they cannot contain **most kinds of** static declarations.
**You cannot declare an interface inside a block; interfaces are inherently static.**

A local class can have static members provided that **they are constant variables**.
The following code excerpt compiles because the static member  `EnglishGoodbye.farewell`  is a constant variable:

    public void sayGoodbyeInEnglish() {
        class EnglishGoodbye {
            public static final String farewell = "Bye bye";
            public void sayGoodbye() {
                System.out.println(farewell);
            }
        }
        EnglishGoodbye myEnglishGoodbye = new EnglishGoodbye();
        myEnglishGoodbye.sayGoodbye();
    }
# Anonymous Classes
## Syntax of Anonymous Classes
Consider the instantiation of the  `frenchGreeting`  object:

        HelloWorld frenchGreeting = new HelloWorld() {
            String name = "tout le monde";
            public void greet() {
                greetSomeone("tout le monde");
            }
            public void greetSomeone(String someone) {
                name = someone;
                System.out.println("Salut " + name);
            }
        };
The anonymous class expression consists of the following:

-   The  `new`  operator
    
-   The name of an **interface** to implement or a **class** to extend. In this example, the anonymous class is implementing the interface  `HelloWorld`.
    
-   Parentheses that contain the arguments to a constructor, just like a normal class instance creation expression.  **Note**: When you implement an interface, there is no constructor, so you use an empty pair of parentheses, as in this example.
    
-   A body, which is a class declaration body. More specifically, in the body, method declarations are allowed but statements are not.
- ## Accessing Local Variables of the Enclosing Scope, and Declaring and Accessing Members of the Anonymous Class
Like local classes, anonymous classes can  [capture variables](https://docs.oracle.com/javase/tutorial/java/javaOO/localclasses.html#accessing-members-of-an-enclosing-class); they have the same access to local variables of the enclosing scope:

-   An anonymous class has access to the members of its enclosing class.
    
-   An anonymous class cannot access local variables in its enclosing scope that are not declared as  `final`  or effectively final.
    
-   Like a nested class, a declaration of a type (such as a variable) in an anonymous class shadows any other declarations in the enclosing scope that have the same name. See  [Shadowing](https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html#shadowing)  for more information.
    

Anonymous classes also have the same restrictions as local classes with respect to their members:

-   You cannot declare static initializers or member interfaces in an anonymous class.
    
-   An anonymous class can have static members provided that they are constant variables.
    

Note that you can declare the following in anonymous classes:

-   Fields
    
-   Extra methods (even if they do not implement any methods of the supertype)
    
-   Instance initializers
    
-   Local classes
    

However, you cannot declare constructors in an anonymous class.
# Lambda Expressions
## Syntax of Lambda Expressions
A lambda expression consists of the following:

- A comma-separated list of formal parameters enclosed in parentheses.
**Note**: You can omit the data type of the parameters in a lambda expression. In addition, you can omit the parentheses if there is only one parameter.
- The arrow token, `->`
- A body, which consists of a single expression or a statement block.
- A return statement is not an expression; in a lambda expression, you must enclose statements in braces (`{}`). However, you do not have to enclose a void method invocation in braces. For example, the following is a valid lambda expression:
`email -> System.out.println(email)`
## Accessing Local Variables of the Enclosing Scope
Like local and anonymous classes, lambda expressions can [capture variables](https://docs.oracle.com/javase/tutorial/java/javaOO/localclasses.html#accessing-members-of-an-enclosing-class); they have the same access to local variables of the enclosing scope. However, unlike local and anonymous classes, lambda expressions do not have any shadowing issues.

# Interfaces
## Defining an Interface
An interface declaration consists of modifiers, the keyword `interface`, the interface name, a comma-separated list of parent interfaces (if any), and the interface body.
## The Interface Body
The interface body can contain  [abstract methods](https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html),  [default methods](https://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html), and  [static methods](https://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html#static). An abstract method within an interface is followed by a semicolon, but no braces (an abstract method does not contain an implementation). Default methods are defined with the  `default`  modifier, and static methods with the  `static`  keyword. All abstract, default, and static methods in an interface are implicitly  `public`, so you can omit the  `public`  modifier.

In addition, an interface can contain constant declarations. All constant values defined in an interface are implicitly  `public`,  `static`, and  `final`. Once again, you can omit these modifiers.
如果想在已实现过的接口中定义新的方法，若干再使用抽象方法，那么实现这个接口的类会出错，都需要要修改。此时使用`default`关键字即可，这样实现接口的类就可以直接使用这个方法了。
# Inheritance
## Private Members in a Superclass
A subclass does not inherit the  `private`  members of its parent class. However, if the superclass has public or protected methods for accessing its private fields, these can also be used by the subclass.

A nested class has access to all the private members of its enclosing class—both fields and methods. Therefore, a public or protected nested class inherited by a subclass has indirect access to all of the private members of the superclass.
## Overriding and Hiding Methods
### Interface Methods
[Default methods](https://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)  and  [abstract methods](https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html)  in interfaces are inherited like instance methods. However, when the supertypes of a class or interface provide multiple default methods with the same signature, the Java compiler follows inheritance rules to resolve the name conflict. These rules are driven by the following two principles:

-   Instance methods are preferred over interface default methods.
- Methods that are already overridden by other candidates are ignored. This circumstance can arise when supertypes share a common ancestor.

If two or more independently defined default methods conflict, or a default method conflicts with an abstract method, then the Java compiler produces a compiler error. You must explicitly override the supertype methods.
A class that implements both `OperateCar` and `FlyCar` must override the method `startEngine`. You could invoke any of the of the default implementations with the `super` keyword.

    public class FlyingCar implements OperateCar, FlyCar {
        // ...
        public int startEngine(EncryptedKey key) {
            FlyCar.super.startEngine(key);
            OperateCar.super.startEngine(key);
        }
    }
Inherited instance methods from classes can override abstract interface methods.
**Note**: Static methods in interfaces are never inherited.
[看代码帮助理解](https://docs.oracle.com/javase/tutorial/java/IandI/override.html)
# 虚拟方法调用(Virtual Method Invocation)
### 1. 什么是虚拟方法

**正常的方法调用：**

    Person e = new Person();
    e.getInfo();
    Student e = new Student();
    e.getInfo();

**虚拟方法调用(多态情况下)：**

    Person e = new Student();
    e.getInfo(); //调用Student类的getInfo()方法

**编译时类型和运行时类型：**

编译时e为Person类型，而方法的调用是在运行时确定的，所以调用的是Student类的getInfo()方法。

——动态绑定

### 2. 多态小结

**前提：**

需要存在继承或者实现关系

要有覆盖操作

**成员方法：**

编译时：要查看引用变量所属的类中是否有所调用的方法。

(编译时检查父类类型)

运行时：调用实际对象所属的类中的重写方法。

(运行时执行子类类型)
**成员变量：**

不具备多态性，只看引用变量所属的类。
# [Object as a Superclass](https://docs.oracle.com/javase/tutorial/java/IandI/objectclass.html)
# Writing Final Classes and Methods
Methods called from constructors should generally be declared final. If a constructor calls a non-final method, a subclass may redefine that method with surprising or undesirable results.
# Abstract Methods and Classes
An  _abstract class_  is a class that is declared  `abstract`—it may or may not include abstract methods. Abstract classes cannot be instantiated, but they can be subclassed.

An  _abstract method_  is a method that is declared without an implementation (without braces, and followed by a semicolon)
If a class includes abstract methods, then the class itself  _must_  be declared  `abstract`, as in:

    public abstract class GraphicObject {
       // declare fields
       // declare nonabstract methods
       abstract void draw();
    }
When an abstract class is subclassed, the subclass usually provides implementations for all of the abstract methods in its parent class. However, if it does not, then the subclass must also be declared `abstract`.

**Note:** Methods in an _interface_ (see the [Interfaces](https://docs.oracle.com/javase/tutorial/java/IandI/createinterface.html) section) that are not declared as default or static are _implicitly_ abstract, so the `abstract` modifier is not used with interface methods. (It can be used, but it is unnecessary.)
## Abstract Classes Compared to Interfaces
Abstract classes are similar to interfaces. You cannot instantiate them, and they may contain a mix of methods declared with or without an implementation. However, with abstract classes, you can declare fields that are not static and final, and define public, protected, and private concrete methods. With interfaces, all fields are automatically public, static, and final, and all methods that you declare or define (as default methods) are public. In addition, you can extend only one class, whether or not it is abstract, whereas you can implement any number of interfaces.

Which should you use, abstract classes or interfaces?

-   Consider using abstract classes if any of these statements apply to your situation:
    -   You want to share code among several closely related classes.
    -   You expect that classes that extend your abstract class have many common methods or fields, or require access modifiers other than public (such as protected and private).
    -   You want to declare non-static or non-final fields. This enables you to define methods that can access and modify the state of the object to which they belong.
-   Consider using interfaces if any of these statements apply to your situation:
    -   You expect that unrelated classes would implement your interface. For example, the interfaces  [`Comparable`](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html)  and  [`Cloneable`](https://docs.oracle.com/javase/8/docs/api/java/lang/Cloneable.html)  are implemented by many unrelated classes.
    -   You want to specify the behavior of a particular data type, but not concerned about who implements its behavior.
    -   You want to take advantage of multiple inheritance of type.
 ## When an Abstract Class Implements an Interface
 In the section on  [`Interfaces`](https://docs.oracle.com/javase/tutorial/java/IandI/createinterface.html), it was noted that a class that implements an interface must implement  _all_  of the interface's methods. It is possible, however, to define a class that does not implement all of the interface's methods, provided that the class is declared to be  `abstract`. For example,

    abstract class X implements Y {
      // implements all but one method of Y
    }
    class XX extends X {
      // implements the remaining method in Y
    }

In this case, class  `X`  must be  `abstract`  because it does not fully implement  `Y`, but class  `XX`  does, in fact, implement  `Y`.
## Class Members
An abstract class may have `static` fields and `static` methods. You can use these static members with a class reference (for example, `AbstractClass.staticMethod()`) as you would with any other class.
# Generics
A  _generic class_  is defined with the following format:

    class name<T1, T2, ..., Tn> { /* ... */ }

The type parameter section, delimited by angle brackets (<>), follows the class name. It specifies the  _type parameters_  (also called  _type variables_)  T1,  T2, ..., and  Tn.
## Type Parameter Naming Conventions
The most commonly used type parameter names are:

-   E - Element (used extensively by the Java Collections Framework)
-   K - Key
-   N - Number
-   T - Type
-   V - Value
-   S,U,V etc. - 2nd, 3rd, 4th types
- ## The Diamond

In Java SE 7 and later, you can replace the type arguments required to invoke the constructor of a generic class with an empty set of type arguments (<>) as long as the compiler can determine, or infer, the type arguments from the context. This pair of angle brackets, <>, is informally called  _the diamond_. For example, you can create an instance of  Box<Integer>  with the following statement:

Box<Integer> integerBox = new Box<>();

For more information on diamond notation and type inference, see  [Type Inference](https://docs.oracle.com/javase/tutorial/java/generics/genTypeInference.html).
**Type Parameter and Type Argument Terminology:** Many developers use the terms "type parameter" and "type argument" interchangeably, but these terms are not the same. When coding, one provides type arguments in order to create a parameterized type. Therefore, the T in Foo<T> is a type parameter and the String in Foo<String> f is a type argument. This lesson observes this definition when using these terms.
## Bounded Type Parameters
To declare a bounded type parameter, list the type parameter's name, followed by the `extends` keyword

     public <U extends Number> void inspect(U u){
            System.out.println("T: " + t.getClass().getName());
            System.out.println("U: " + u.getClass().getName());
        }
## Multiple Bounds
The preceding example illustrates the use of a type parameter with a single bound, but a type parameter can have  _multiple bounds_:

    <T extends B1 & B2 & B3>

A type variable with multiple bounds is a subtype of all the types listed in the bound. If one of the bounds is a class, it must be specified first.
## Generics, Inheritance, and Subtypes

**Note:** Given two concrete types A and B (for example, Number and Integer), MyClass< A> has no relationship to MyClass< B>, regardless of whether or not A and B are related. The common parent of MyClass< A> and MyClass< B> is Object.
You can subtype a generic class or interface by extending or implementing it. The relationship between the type parameters of one class or interface and the type parameters of another are determined by the  extends  and  implements  clauses.

Using the  Collections  classes as an example,  ArrayList< E>  implements  List<E>, and  List< E> extends Collection<E>. So  ArrayList< String>  is a subtype of  List< String>, which is a subtype of  Collection< String>. So long as you do not vary the type argument, the subtyping relationship is preserved between the types.

Now imagine we want to define our own list interface,  PayloadList, that associates an optional value of generic type  P  with each element. Its declaration might look like:

    interface PayloadList<E,P> extends List<E> {
      void setPayload(int index, P val);
      ...
    }
**Note:** 只要E类型一样就行
## Type Inference and Generic Constructors of Generic and Non-Generic Classes
Note that constructors can be generic (in other words, declare their own formal type parameters) in both generic and non-generic classes. Consider the following example:

    class MyClass<X> {
      <T> MyClass(T t) {
        // ...
      }
    }
## Wildcards
In generic code, the question mark (?), called the _wildcard_, represents an unknown type. The wildcard can be used in a variety of situations: as the type of a parameter, field, or local variable; sometimes as a return type (though it is better programming practice to be more specific). The wildcard is never used as a type argument for a generic method invocation, a generic class instance creation, or a supertype.
### Upper Bounded Wildcards

    List<? extends Number>
### Unbounded Wildcards
The unbounded wildcard type is specified using the wildcard character (?), for example,  List<?>. This is called a  _list of unknown type_. There are two scenarios where an unbounded wildcard is a useful approach:

-   If you are writing a method that can be implemented using functionality provided in the  Object  class.
-   When the code is using methods in the generic class that don't depend on the type parameter. For example,  List.size  or  List.clear. In fact,  Class<?>  is so often used because most of the methods in  Class<T>  do not depend on  T.
### Lower Bounded Wildcards
A lower bounded wildcard is expressed using the wildcard character ('?'), following by the super keyword, followed by its _lower bound_: `<? super A>`.
**Note:** You can specify an upper bound for a wildcard, or you can specify a lower bound, but you cannot specify both.
### Wildcard Capture and Helper Methods
[看代码理解](https://docs.oracle.com/javase/tutorial/java/generics/capture.html)
### Guidelines for Wildcard Use
**Wildcard Guidelines:**

-   An "in" variable is defined with an upper bounded wildcard, using the  extends  keyword.
-   An "out" variable is defined with a lower bounded wildcard, using the  super  keyword.
-   In the case where the "in" variable can be accessed using methods defined in the  Object  class, use an unbounded wildcard.
-   In the case where the code needs to access the variable as both an "in" and an "out" variable, do not use a wildcard.

These guidelines do not apply to a method's return type. Using a wildcard as a return type should be avoided because it forces programmers using the code to deal with wildcards.
[**Wildcard Guidelines**](https://docs.oracle.com/javase/tutorial/java/generics/wildcardGuidelines.html)
### Type Erasure
[参考](https://www.jianshu.com/p/f9da328c91be)
### Restrictions on Generics
To use Java generics effectively, you must consider the following restrictions:

-   [Cannot Instantiate Generic Types with Primitive Types](https://docs.oracle.com/javase/tutorial/java/generics/restrictions.html#instantiate)
-   [Cannot Create Instances of Type Parameters](https://docs.oracle.com/javase/tutorial/java/generics/restrictions.html#createObjects)
-   [Cannot Declare Static Fields Whose Types are Type Parameters](https://docs.oracle.com/javase/tutorial/java/generics/restrictions.html#createStatic)
-   [Cannot Use Casts or  instanceof  With Parameterized Types](https://docs.oracle.com/javase/tutorial/java/generics/restrictions.html#cannotCast)
-   [Cannot Create Arrays of Parameterized Types](https://docs.oracle.com/javase/tutorial/java/generics/restrictions.html#createArrays)
-   [Cannot Create, Catch, or Throw Objects of Parameterized Types](https://docs.oracle.com/javase/tutorial/java/generics/restrictions.html#cannotCatch)
-   [Cannot Overload a Method Where the Formal Parameter Types of Each Overload Erase to the Same Raw Type](https://docs.oracle.com/javase/tutorial/java/generics/restrictions.html#cannotOverload)
# Basic I/O
## Byte Streams
Programs use _byte streams_ to perform input and output of 8-bit bytes. All byte stream classes are descended from [`InputStream`](https://docs.oracle.com/javase/8/docs/api/java/io/InputStream.html) and [`OutputStream`](https://docs.oracle.com/javase/8/docs/api/java/io/OutputStream.html).
### When Not to Use Byte Streams

`CopyBytes`  seems like a normal program, but it actually represents a kind of low-level I/O that you should avoid. Since  `xanadu.txt`  contains character data, the best approach is to use  [character streams](https://docs.oracle.com/javase/tutorial/essential/io/charstreams.html), as discussed in the next section. There are also streams for more complicated data types. Byte streams should only be used for the most primitive I/O.
So why talk about byte streams? Because all other stream types are built on byte streams.
## Character Streams
All character stream classes are descended from [`Reader`](https://docs.oracle.com/javase/8/docs/api/java/io/Reader.html) and [`Writer`](https://docs.oracle.com/javase/8/docs/api/java/io/Writer.html). As with byte streams, there are character stream classes that specialize in file I/O: [`FileReader`](https://docs.oracle.com/javase/8/docs/api/java/io/FileReader.html) and [`FileWriter`](https://docs.oracle.com/javase/8/docs/api/java/io/FileWriter.html).
### Character Streams that Use Byte Streams
Character streams are often "wrappers" for byte streams. The character stream uses the byte stream to perform the physical I/O, while the character stream handles translation between characters and bytes.  `FileReader`, for example, uses  `FileInputStream`, while  `FileWriter`  uses  `FileOutputStream`.

There are two general-purpose byte-to-character "bridge" streams:  [`InputStreamReader`](https://docs.oracle.com/javase/8/docs/api/java/io/InputStreamReader.html)  and  [`OutputStreamWriter`](https://docs.oracle.com/javase/8/docs/api/java/io/OutputStreamWriter.html). Use them to create character streams when there are no prepackaged character stream classes that meet your needs. The  [sockets lesson](https://docs.oracle.com/javase/tutorial/networking/sockets/readingWriting.html)  in the  [networking trail](https://docs.oracle.com/javase/tutorial/networking/index.html)  shows how to create character streams from the byte streams provided by socket classes.
## Buffered Streams
Most of the examples we've seen so far use  _unbuffered_  I/O. This means each read or write request is handled directly by the underlying OS. This can make a program much less efficient, since each such request often triggers disk access, network activity, or some other operation that is relatively expensive.

To reduce this kind of overhead, the Java platform implements  _buffered_  I/O streams. Buffered input streams read data from a memory area known as a  _buffer_; the native input API is called only when the buffer is empty. Similarly, buffered output streams write data to a buffer, and the native output API is called only when the buffer is full.

A program can convert an unbuffered stream into a buffered stream using the wrapping idiom we've used several times now, where the unbuffered stream object is passed to the constructor for a buffered stream class. Here's how you might modify the constructor invocations in the  `CopyCharacters`  example to use buffered I/O:

inputStream = new BufferedReader(new FileReader("xanadu.txt"));
outputStream = new BufferedWriter(new FileWriter("characteroutput.txt"));

There are four buffered stream classes used to wrap unbuffered streams:  [`BufferedInputStream`](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedInputStream.html)  and  [`BufferedOutputStream`](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedOutputStream.html)  create buffered byte streams, while  [`BufferedReader`](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedReader.html)  and  [`BufferedWriter`](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedWriter.html)  create buffered character streams.
### Flushing Buffered Streams

It often makes sense to write out a buffer at critical points, without waiting for it to fill. This is known as  _flushing_  the buffer.

Some buffered output classes support  _autoflush_, specified by an optional constructor argument. When autoflush is enabled, certain key events cause the buffer to be flushed.
## Scanning and Formatting
Programming I/O often involves translating to and from the neatly formatted data humans like to work with. To assist you with these chores, the Java platform provides two APIs. The [scanner](https://docs.oracle.com/javase/tutorial/essential/io/scanning.html) API breaks input into individual tokens associated with bits of data. The [formatting](https://docs.oracle.com/javase/tutorial/essential/io/formatting.html) API assembles data into nicely formatted, human-readable form.
### Scanning
#### Breaking Input into Tokens
By default, a scanner uses white space to separate tokens. (White space characters include blanks, tabs, and line terminators).
To use a different token separator, invoke  `useDelimiter()`, specifying a regular expression. For example, suppose you wanted the token separator to be a comma, optionally followed by white space. You would invoke,

    s.useDelimiter(",\\s*");
#### Translating Individual Tokens
The `ScanXan` example treats all input tokens as simple `String` values. `Scanner` also supports tokens for all of the Java language's primitive types (except for `char`), as well as `BigInteger` and `BigDecimal`.
### Formatting
Stream objects that implement formatting are instances of either [`PrintWriter`](https://docs.oracle.com/javase/8/docs/api/java/io/PrintWriter.html), a character stream class, or [`PrintStream`](https://docs.oracle.com/javase/8/docs/api/java/io/PrintStream.html), a byte stream class.

**Note:** The only `PrintStream` objects you are likely to need are [`System.out`](https://docs.oracle.com/javase/8/docs/api/java/lang/System.html#out) and [`System.err`](https://docs.oracle.com/javase/8/docs/api/java/lang/System.html#err). (See [I/O from the Command Line](https://docs.oracle.com/javase/tutorial/essential/io/cl.html) for more on these objects.) When you need to create a formatted output stream, instantiate `PrintWriter`, not `PrintStream`.
Like all byte and character stream objects, instances of  `PrintStream`  and  `PrintWriter`  implement a standard set of  `write`  methods for simple byte and character output. In addition, both  `PrintStream`  and  `PrintWriter`  implement the same set of methods for converting internal data into formatted output. Two levels of formatting are provided:

-   `print`  and  `println`  format individual values in a standard way.
-   `format`  formats almost any number of values based on a format string, with many options for precise formatting.
## I/O from the Command Line
A program is often run from the command line and interacts with the user in the command line environment. The Java platform supports this kind of interaction in two ways: through the Standard Streams and through the Console.

## Standard Streams

Standard Streams are a feature of many operating systems. By default, they read input from the keyboard and write output to the display. They also support I/O on files and between programs, but that feature is controlled by the command line interpreter, not the program.

The Java platform supports three Standard Streams:  _Standard Input_, accessed through  `System.in`;  _Standard Output_, accessed through  `System.out`; and  _Standard Error_, accessed through  `System.err`. These objects are defined automatically and do not need to be opened. Standard Output and Standard Error are both for output; having error output separately allows the user to divert regular output to a file and still be able to read error messages. For more information, refer to the documentation for your command line interpreter.

You might expect the Standard Streams to be character streams, but, for historical reasons, they are byte streams.  `System.out`  and  `System.err`  are defined as  [`PrintStream`](https://docs.oracle.com/javase/8/docs/api/java/io/PrintStream.html)  objects. Although it is technically a byte stream,  `PrintStream`  utilizes an internal character stream object to emulate many of the features of character streams.
By contrast,  `System.in`  is a byte stream with no character stream features. To use Standard Input as a character stream, wrap  `System.in`  in  `InputStreamReader`.

    InputStreamReader cin = new InputStreamReader(System.in);
### The Console

A more advanced alternative to the Standard Streams is the Console. This is a single, predefined object of type  [`Console`](https://docs.oracle.com/javase/8/docs/api/java/io/Console.html)  that has most of the features provided by the Standard Streams, and others besides. The Console is particularly useful for secure password entry. The Console object also provides input and output streams that are true character streams, through its  `reader`  and  `writer`  methods.

Before a program can use the Console, it must attempt to retrieve the Console object by invoking  `System.console()`. If the Console object is available, this method returns it. If  `System.console`  returns  `NULL`, then Console operations are not permitted, either because the OS doesn't support them or because the program was launched in a noninteractive environment.

The Console object supports secure password entry through its  `readPassword`  method. This method helps secure password entry in two ways. First, it suppresses echoing, so the password is not visible on the user's screen. Second,  `readPassword`  returns a character array, not a  `String`, so the password can be overwritten, removing it from memory as soon as it is no longer needed.
## Data Streams
Data streams support binary I/O of primitive data type values (`boolean`, `char`, `byte`, `short`, `int`, `long`, `float`, and `double`) as well as String values. All data streams implement either the [`DataInput`](https://docs.oracle.com/javase/8/docs/api/java/io/DataInput.html) interface or the [`DataOutput`](https://docs.oracle.com/javase/8/docs/api/java/io/DataOutput.html) interface.
`DataStreams`  uses one very bad programming technique: it uses floating point numbers to represent monetary values. In general, floating point is bad for precise values. It's particularly bad for decimal fractions, because common values (such as  `0.1`) do not have a binary representation.

The correct type to use for currency values is  [`java.math.BigDecimal`](https://docs.oracle.com/javase/8/docs/api/java/math/BigDecimal.html). Unfortunately,  `BigDecimal`  is an object type, so it won't work with data streams. However,  `BigDecimal`  _will_  work with object streams
## Object Streams
Just as data streams support I/O of primitive data types, object streams support I/O of objects. Most, but not all, standard classes support serialization of their objects. Those that do implement the marker interface  [`Serializable`](https://docs.oracle.com/javase/8/docs/api/java/io/Serializable.html).

The object stream classes are  [`ObjectInputStream`](https://docs.oracle.com/javase/8/docs/api/java/io/ObjectInputStream.html)  and  [`ObjectOutputStream`](https://docs.oracle.com/javase/8/docs/api/java/io/ObjectOutputStream.html). These classes implement  [`ObjectInput`](https://docs.oracle.com/javase/8/docs/api/java/io/ObjectInput.html)  and  [`ObjectOutput`](https://docs.oracle.com/javase/8/docs/api/java/io/ObjectOutput.html), which are subinterfaces of  `DataInput`  and  `DataOutput`. That means that all the primitive data I/O methods covered in  [Data Streams](https://docs.oracle.com/javase/tutorial/essential/io/datastreams.html)  are also implemented in object streams. So an object stream can contain a mixture of primitive and object values. The  [`ObjectStreams`](https://docs.oracle.com/javase/tutorial/essential/io/examples/ObjectStreams.java)  example illustrates this.  `ObjectStreams`  creates the same application as  `DataStreams`, with a couple of changes. First, prices are now  [`BigDecimal`](https://docs.oracle.com/javase/8/docs/api/java/math/BigDecimal.html)objects, to better represent fractional values. Second, a  [`Calendar`](https://docs.oracle.com/javase/8/docs/api/java/util/Calendar.html)  object is written to the data file, indicating an invoice date.
### Output and Input of Complex Objects
many objects contain references to other objects. If `readObject` is to reconstitute an object from a stream, it has to be able to reconstitute all of the objects the original object referred to. These additional objects might have their own references, and so on.
# [Concurrency](https://docs.oracle.com/javase/tutorial/essential/concurrency/index.html)
## 同步(Synchronization)
线程的通信主要通过共享访问字段和引用字段的对象。这种形式的通信非常有效，但可能产生两种错误:线程干扰( thread interference)和内存一致性错误(memory consistency errors)。防止这些错误所需的工具是同步。
但是，同步可能会引入线程争用，当两个或多个线程试图同时访问同一资源时，会导致Java运行时执行一个或多个线程的速度变慢，甚至暂停它们的执行。饥饿(Starvation)和活锁(livelock)就是是线程争用的具体形式。
## Thread interference
## Memory Consistency Errors
避免内存一致性错误的关键是理解happen-before的关系, 这种关系保证一个特定语句所写的内存对另一个特定语句可见。

有几个操作可以创建happens-before关系，其中之一就是同步。

Java提供了两种同步语义，**synchronized方法**和**synchronized申明**
### synchronized方法
要使一个方法同步，只需在它的声明中添加`synchronized`关键字:
```java
public class SynchronizedCounter {
    private int c = 0;
    public synchronized void increment() {
        c++;
    }
    public synchronized void decrement() {
        c--;
    }
    public synchronized int value() {
        return c;
    }
}
```
synchronized方法很有效，但可能会导致*liveness*问题

当线程调用同步方法时，它会自动获取该方法对象的内在锁，并在该方法返回时释放它。即使返回是由未捕获的异常引起的，也会释放锁。
###  synchronized 关键字
`synchronized`申明必须指定提供内在锁的对象:
```java
public void addName(String name) {
    synchronized(this) {
        lastName = name;
        nameCount++;
    }
    nameList.add(name);
}
```
## Reentrant Synchronization
一个线程不能获得另一个线程拥有的锁。但是线程可以获得它已经拥有的锁。允许一个线程多次获取同一个锁可以实现重入同步。这描述了同步代码(直接或间接地)调用同样包含同步代码的方法，并且两组代码使用相同的锁的情况。如果没有可重入同步，同步代码将不得不采取许多额外的预防措施，以避免线程本身导致阻塞。
## 原子性
原子操作不能交错，因此可以在不用担心线程干扰的情况下使用它们。然而，这并不能消除所有同步原子操作的需要，因为内存一致性错误仍然是可能的。使用volatile变量可以降低内存一致性错误的风险，因为对易失性变量的任何写操作都会与随后对该变量的读操作建立happens-before关系。这意味着对volatile变量的更改对其他线程总是可见的。此外，这还意味着，当线程读取volatile变量时，它不仅可以看到对volatile的最新更改，还可以看到导致更改的代码的副作用。

使用简单的原子变量访问比通过同步代码访问这些变量更有效，但需要程序员更加注意避免内存一致性错误。额外的努力是否值得取决于应用程序的大小和复杂性。
## 死锁 (Deadlock)
死锁就是多个线程一直阻塞，等待对方释放锁的情景。
## Starvation 和 Livelock
饥饿和活锁相比死锁是不常见的问题，但是仍然是每个并发软件的设计者可能遇到的问题。
- 饥饿描述了这样一种情况:线程无法获得对共享资源的常规访问，无法取得进展。当“贪婪”线程使共享资源长时间不可用时，就会发生这种情况。例如，假设一个对象提供了一个通常需要很长时间才能返回的同步方法。如果一个线程经常调用这个方法，那么其他需要频繁同步访问同一对象的线程经常会被阻塞。
- 一个线程经常响应另一个线程的操作。如果另一个线程的操作也是对另一个线程的操作的响应，那么可能会产生livellock。与死锁一样，被活锁的线程无法取得进一步进展。然而，线程并没有被阻塞——它们只是忙于响应彼此而无法继续工作。

## 高级并发对象
同步代码依赖于一种简单的可重入锁。这种锁使用方便，但有很多限制。java.util.concurrent支持更复杂的锁定习惯用法。

锁对象的工作原理与同步代码使用的隐式锁非常相似。与隐式锁一样，一次只能有一个线程拥有一个锁对象。锁对象还通过其关联的条件对象支持等待/通知机制。

锁对象相对于隐式锁的最大优势是它们能够退出获取锁的尝试。如果锁不是立即可用的，或者在超时过期之前(如果指定了)，tryLock方法将退出。如果另一个线程在获得锁之前发送了一个中断，那么lockInterruptibly方法就会退出。

## 创建一个线程

Java 提供了三种创建线程的方法：

-   通过实现 Runnable 接口；
-   通过继承 Thread 类本身；
-   通过 Callable 和 Future 创建线程。
## 通过 Callable 和 Future 创建线程

-   1. 创建 Callable 接口的实现类，并实现 call() 方法，该 call() 方法将作为线程执行体，并且有返回值。
    
-   2. 创建 Callable 实现类的实例，使用 FutureTask 类来包装 Callable 对象，该 FutureTask 对象封装了该 Callable 对象的 call() 方法的返回值。
    
-   3. 使用 FutureTask 对象作为 Thread 对象的 target 创建并启动新线程。
    
-   4. 调用 FutureTask 对象的 get() 方法来获得子线程执行结束后的返回值。
## 创建线程的三种方式的对比

-   1. 采用实现 Runnable、Callable 接口的方式创建多线程时，线程类只是实现了 Runnable 接口或 Callable 接口，还可以继承其他类。
    
-   2. 使用继承 Thread 类的方式创建多线程时，编写简单，如果需要访问当前线程，则无需使用 Thread.currentThread() 方法，直接使用 this 即可获得当前线程。

## 使用线程池
1. Create a task(Runnable Object) to execute
```
class Task implements Runnable{}
Runnable r1 = new Task("task 1");
```
2. Create Executor Pool using Executors
`ExecutorService pool = Executors.newFixedThreadPool(MAX_T);`
3. Pass tasks to Executor Pool
`pool.execute(r1);`
4. Shutdown the Executor Pool
`pool.shutdown();`
## [自定义线程池](https://www.cnblogs.com/nele/p/6502750.html)
使用`java.uitl.concurrent.ThreadPoolExecutor`类:

    ThreadPoolExecutor pool = new ThreadPoolExecutor(paremeters)

# Reflection
## Uses of Reflection

Reflection is commonly used by programs which require the ability to examine or modify the runtime behavior of applications running in the Java virtual machine. This is a relatively advanced feature and should be used only by developers who have a strong grasp of the fundamentals of the language. With that caveat in mind, reflection is a powerful technique and can enable applications to perform operations which would otherwise be impossible.

Extensibility Features

An application may make use of external, user-defined classes by creating instances of extensibility objects using their fully-qualified names.

Class Browsers and Visual Development Environments

A class browser needs to be able to enumerate the members of classes. Visual development environments can benefit from making use of type information available in reflection to aid the developer in writing correct code.

Debuggers and Test Tools

Debuggers need to be able to examine private members on classes. Test harnesses can make use of reflection to systematically call a discoverable set APIs defined on a class, to insure a high level of code coverage in a test suite.

## Drawbacks of Reflection

Reflection is powerful, but should not be used indiscriminately. If it is possible to perform an operation without using reflection, then it is preferable to avoid using it. The following concerns should be kept in mind when accessing code via reflection.

**Performance Overhead**

Because reflection involves types that are dynamically resolved, certain Java virtual machine optimizations can not be performed. Consequently, reflective operations have slower performance than their non-reflective counterparts, and should be avoided in sections of code which are called frequently in performance-sensitive applications.

**Security Restrictions**

Reflection requires a runtime permission which may not be present when running under a security manager. This is in an important consideration for code which has to run in a restricted security context, such as in an Applet.

**Exposure of Internals**

Since reflection allows code to perform operations that would be illegal in non-reflective code, such as accessing  `private`  fields and methods, the use of reflection can result in unexpected side-effects, which may render code dysfunctional and may destroy portability. Reflective code breaks abstractions and therefore may change behavior with upgrades of the platform.
## Retrieving Class Objects
### Object.getClass()
If an instance of an object is available, then the simplest way to get its [`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) is to invoke [`Object.getClass()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#getClass--). Of course, this only works for reference types which all inherit from [`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html).
### The .class Syntax
If the type is available but there is no instance then it is possible to obtain a  [`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)  by appending  `".class"`  to the name of the type. This is also the easiest way to obtain the  [`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)  for a primitive type.

    boolean b;
    Class c = b.getClass();   // compile-time error   
    Class c = boolean.class;  // correct
### Class.forName()

If the fully-qualified name of a class is available, it is possible to get the corresponding  [`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)  using the static method  [`Class.forName()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#forName-java.lang.String-). This cannot be used for primitive types. The syntax for names of array classes is described by  [`Class.getName()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getName--). This syntax is applicable to references and primitive types.
## TYPE Field for Primitive Type Wrappers

The  `.class`  syntax is a more convenient and the preferred way to obtain the  [`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)  for a primitive type; however there is another way to acquire the  [`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html). Each of the primitive types and  `void`  has a wrapper class in  [`java.lang`](https://docs.oracle.com/javase/8/docs/api/java/lang/package-summary.html)  that is used for boxing of primitive types to reference types. Each wrapper class contains a field named  `TYPE`  which is equal to the  [`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)  for the primitive type being wrapped.

    Class c = Double.TYPE;
### Methods that Return Classes
[Link](https://docs.oracle.com/javase/tutorial/reflect/class/classNew.html)
## Examining Class Modifiers and Types
A class may be declared with one or more modifiers which affect its runtime behavior:

-   Access modifiers:  `public`,  `protected`, and  `private`
-   Modifier requiring override:  `abstract`
-   Modifier restricting to one instance:  `static`
-   Modifier prohibiting value modification:  `final`
-   Modifier forcing strict floating point behavior:  `strictfp`
-   Annotations

Not all modifiers are allowed on all classes, for example an interface cannot be `final` and an enum cannot be `abstract`. [`java.lang.reflect.Modifier`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Modifier.html) contains declarations for all possible modifiers. It also contains methods which may be used to decode the set of modifiers returned by [`Class.getModifiers()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getModifiers--).
# Discovering Class Members

There are two categories of methods provided in  [`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)  for accessing fields, methods, and constructors: methods which enumerate these members and methods which search for particular members. Also there are distinct methods for accessing members declared directly on the class versus methods which search the superinterfaces and superclasses for inherited members.
[Link](https://docs.oracle.com/javase/tutorial/reflect/class/classMembers.html)
## Troubleshooting
[Link](https://docs.oracle.com/javase/tutorial/reflect/class/classTrouble.html)
# 面试问题
## [集合框架](https://www.processon.com/diagraming/5ec10c435653bb6f2a14430f)
## Collection接口
代表一组对象，有序或无序，重复或不重复。

有三个子接口: `List`, `Set`, `Queue`
### List接口
- List接口可以精确地控制每个元素在List中地插入位置，可以通过索引来获取里面地元素。
- 与Set不同的是List可以有重复的元素。并且在允许有Null值的List中(有的List不允许有null值)，允许多个Null值。
- List提供了特殊的迭代器，除了itertor的普通操作外，还允许插入和替换元素，以及双向访问。
#### ArrayList
- 使用可变数组实现List接口，允许任何类型的元素包括null
- ArrayList大致相当于Vector，不同的是它是不同步的unsynchronized; 如果多个线程同时访问一个Arraylist实例，并且至少有一个线程在结构上修改了这个List，那么它必须在外部同步。
- 要实现List的线程安全,可以使用 `List list = Collections.synchronizedList(new ArrayList(...))`
#### Vector
- 如果不需要实现线程安全，建议用ArrayList代替Vector.
- 在结构上和功能上没有什么区别，源码中在这些类的关键操作函数中，Vector都加上了synchronized关键字
- Vector扩容为原来的2倍长度，ArrayList扩容为原来1.5倍
### AbstractSequentialList
- 继承AbstractList，只支持按顺序访问。
- AbstractSequentiaList和RandomAccess主要的区别是AbstractSequentiaList的主要方法都是通过迭代器实现的，而不是直接实现的
#### ListedList
- 继承AbstractSequentiaList类，实现了List, Deque接口，基于双链表实现，允许所有元素包括null值
- 不是同步的， 如果多个线程同时访问一个Arraylist实例，并且至少有一个线程在结构上修改了这个List，那么它必须在外部同步。
- 要实现List的线程安全,可以使用 `List list = Collections.synchronizedList(new LinkedList(...))`
### Set 接口
- 不允许有重复元素，最多包含一个null值，有些实现不允许有null值
#### Treeset
- 是一个基于TreeMap的NavigableSet的实现
- 不是线程同步的，使用`SortedSet s = Collections.synchronizedSortedSet(new TreeSet(...))`实现同步
#### HashSet
- 实现了Set接口，由HashMap支持，它不保证集合的迭代顺序不变和元素随时间变化而不变，允许由null值
- 不是线程同步的，使用`Set s = Collections.synchronizedSet(new HashSet(....))`实现同步
##### LinkedHashSet
- 一种Hash表和链表对Set接口的实现
- 与HashSet不同的是，它通过在entry上维护一个双列表，这个链表定义了迭代顺序，通常是key插入到Set中的顺序。
## Map接口
- 是一个将keys映射到value的对象，不允许由重复的keys，每个key至多有一个value
- map的顺序定义为map集合视图上的迭代器返回元素的顺序，TreeMap对顺序做了保证，HashMap则没有
- 不允许将自己作为key，虽然允许将自己作为value，但不建议这样。
- 有些实现对key和value做出了限制，如禁止空key和value，有些对key的类型做出了限制
### TreeMap
- 是一个基于红黑树的NavigableMap的实现。
- 是不同步的，使用`SortedMap m = Collections.synchronizedSortedMap(new TreeMap(...))`实现同步

### HashMap
- 基于Hash表的Map接口实现，允许null值和null键
- HashMap相当于`hashtable`类，不同的是它是不同步的并且允许null值，使用`Mapp m = Collections.synchronizedMap(new HashMap(...));`实现同步
- 不保证有序
#### LinkedHashMap
- 一种Hash表和链表对Map接口的实现
- 与Hashtable不同的是，它通过在entry上维护一个双列表，这个链表定义了迭代顺序，通常是key插入到map中的顺序。
- 不同步,使用`Mapp m = Collections.synchronizedMap(new LinkedHashMap(...));`实现同步
- 这种技术在一个模块接受map并复制它，然后返回由复制时决定的顺序这样的场景非常有用；非常适合构建LRU缓存。

> 看实现hash表、红黑树部分的代码
# Java 锁机制
在Java中，每个对象都有一把锁，这个锁存放在对象头中，锁中记录了当前对象被哪个线程锁占用。Java对象包含三个部分，对象头-实例数据-填充字节。对象头存放了对象的运行时信息，其中包括**Mark Word**和**Class pointer**. Class point指向了当前对象类型所在方法区中的类型数据。Mark Word存储了很多和当前对象运行时的状态有关的数据，比如hashcode,锁状态标志。

![mark word](../resources/Lock.png)

锁状态包括四种
- 无锁：不对资源进行锁定，所有线程可以访问对象，会出现无竞争和有竞争两种情况。有竞争时不使用锁进行控制多线程，通过限制使同时只有一个线程访问资源，其他修改失败的线程不断重试，这就是**CAS**.

- 偏向锁: 当对象加锁时，实际运行中只有一个线程获得该对象锁，最理想的方式是不通过线程切换和不通过CAS来获得锁。让这个对象可以认识这个线程，只要这个线程过来，对象就直接把锁交出去，这就是偏向锁。
**实现方法：**在Mark Word中当锁标志位位01时，判断倒数第三个bit是否为1，是1则为偏向锁，否则为无所。如果锁状态为无锁，那么再读Mark Word的前23个bit,这23个bit的值就是线程ID.通过线程ID来确认当前想要获得对象锁的线程是不是老顾客。假如对象发现不只有一个线程而是有多个线程正在竞争锁，那么偏向锁会升级为轻量级锁。

- 轻量级锁  
当锁状态升级为轻量级锁时，mark word中前30个bit是*指向线程栈中锁记录的指针*。当一个线程向获得某个对象的锁时，看到锁标志位为00时就知道它是轻量级锁。这是线程会在自己的虚拟机栈中开辟一块称为**Lock Record**的空间。Lock Record中存放的是对象头的*Mark Record的副本*以及*Owner指针*。线程通过CAS去尝试获取锁，一但获取锁就会复制该对象头中的Mark Record到Lock Record中，并且将Lock Record中的owner指针指向该对象。另一方面，对象中Mark Record的前30bit将会生成一个指针，指向线程虚拟机栈中的Lock Record，这样就实现了线程与对象的绑定，互相知道了对方的存在。对象被绑定后，获取这个对象锁的线程就可以执行任务，其他想要获取这个对象的线程会自旋等待，自旋就是不断循环看锁有没有被释放。如果自旋等待的线程超过一个，轻量级锁将会升级为重量级锁。
- 重量级锁
如果升级为重量级锁，需要通过monitor来对线程进行控制，此时将会完全锁定资源

只能升级不能降级
`synchronized`可以用来同步线程，`synchronized`被编译后会生成`monitorenter`和`monitorexit`两个字节码指令，依赖这两个字节码进行线程同步。一个线程进入monitor，其他线程只能等待
