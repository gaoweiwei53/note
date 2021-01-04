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
# Concurrency
## Processes
A process has a self-contained execution environment. A process generally has a complete, private set of basic run-time resources; in particular, each process has its own memory space.
## Threads
Threads are sometimes called  _lightweight processes_. Both processes and threads provide an execution environment, but creating a new thread requires fewer resources than creating a new process.

Threads exist within a process — every process has at least one. Threads share the process's resources, including memory and open files. This makes for efficient, but potentially problematic, communication.
## Thread Objects
Each thread is associated with an instance of the class  [`Thread`](https://docs.oracle.com/javase/8/docs/api/java/lang/Thread.html). There are two basic strategies for using  `Thread`  objects to create a concurrent application.

-   To directly control thread creation and management, simply instantiate  `Thread`  each time the application needs to initiate an asynchronous task.
-   To abstract thread management from the rest of your application, pass the application's tasks to an  _executor_.

This section documents the use of  `Thread`  objects. Executors are discussed with other  [high-level concurrency objects](https://docs.oracle.com/javase/tutorial/essential/concurrency/highlevel.html).
## Defining and Starting a Thread
An application that creates an instance of `Thread` must provide the code that will run in that thread. There are two ways to do this:
- _Provide a  `Runnable`  object._ The [`Runnable`](https://docs.oracle.com/javase/8/docs/api/java/lang/Runnable.html) interface defines a single method, `run`, meant to contain the code executed in the thread. The `Runnable` object is passed to the `Thread` constructor.
```
 public class HelloRunnable implements Runnable {
    public void run() {
        System.out.println("Hello from a thread!");
    }
    public static void main(String args[]) {
        (new Thread(new HelloRunnable())).start();
    }
}
```
- _Subclass  `Thread`._ The `Thread` class itself implements `Runnable`, though its `run` method does nothing. An application can subclass `Thread`, providing its own implementation of `run`
```
public class HelloThread extends Thread {

    public void run() {
        System.out.println("Hello from a thread!");
    }

    public static void main(String args[]) {
        (new HelloThread()).start();
    }

}
```
Which of these idioms should you use? The first idiom, which employs a  `Runnable`  object, is more general, because the  `Runnable`  object can subclass a class other than  `Thread`. The second idiom is easier to use in simple applications, but is limited by the fact that your task class must be a descendant of  `Thread`. This lesson focuses on the first approach, which separates the  `Runnable`  task from the  `Thread`  object that executes the task. Not only is this approach more flexible, but it is applicable to the high-level thread management APIs covered later.

The  `Thread`  class defines a number of methods useful for thread management. These include  `static`  methods, which provide information about, or affect the status of, the thread invoking the method. The other methods are invoked from other threads involved in managing the thread and  `Thread`  object. We'll examine some of these methods in the following sections.
## Pausing Execution with Sleep
`Thread.sleep` causes the current thread to suspend execution for a specified period. This is an efficient means of making processor time available to the other threads of an application or other applications that might be running on a computer system.
## Interrupts
An  _interrupt_  is an indication to a thread that it should stop what it is doing and do something else. It's up to the programmer to decide exactly how a thread responds to an interrupt, but it is very common for the thread to terminate. This is the usage emphasized in this lesson.

A thread sends an interrupt by invoking  [`interrupt`](https://docs.oracle.com/javase/8/docs/api/java/lang/Thread.html#interrupt--)  on the  `Thread`  object for the thread to be interrupted. For the interrupt mechanism to work correctly, the interrupted thread must support its own interruption.
### The Interrupt Status Flag
The interrupt mechanism is implemented using an internal flag known as the  _interrupt status_. Invoking  `Thread.interrupt`  sets this flag. When a thread checks for an interrupt by invoking the static method  `Thread.interrupted`, interrupt status is cleared. The non-static  `isInterrupted`  method, which is used by one thread to query the interrupt status of another, does not change the interrupt status flag.

By convention, any method that exits by throwing an  `InterruptedException`  clears interrupt status when it does so. However, it's always possible that interrupt status will immediately be set again, by another thread invoking  `interrupt`.
## Joins
The  `join`  method allows one thread to wait for the completion of another. If  `t`  is a  `Thread`  object whose thread is currently executing,

    t.join();
causes the current thread to pause execution until  `t`'s thread terminates. Overloads of  `join`  allow the programmer to specify a waiting period. However, as with  `sleep`,  `join`  is dependent on the OS for timing, so you should not assume that  `join`  will wait exactly as long as you specify.

Like  `sleep`,  `join`  responds to an interrupt by exiting with an  `InterruptedException`.
## Synchronization
Threads communicate primarily by sharing access to fields and the objects reference fields refer to. This form of communication is extremely efficient, but makes two kinds of errors possible:  _thread interference_  and  _memory consistency errors_. The tool needed to prevent these errors is  _synchronization_.

However, synchronization can introduce  _thread contention_, which occurs when two or more threads try to access the same resource simultaneously  _and_  cause the Java runtime to execute one or more threads more slowly, or even suspend their execution.  [Starvation and livelock](https://docs.oracle.com/javase/tutorial/essential/concurrency/starvelive.html)  are forms of thread contention. See the section  [Liveness](https://docs.oracle.com/javase/tutorial/essential/concurrency/liveness.html)  for more information.
### Thread Interference
### Memory Consistency Errors
_Memory consistency errors_  occur when different threads have inconsistent views of what should be the same data. The causes of memory consistency errors are complex and beyond the scope of this tutorial. Fortunately, the programmer does not need a detailed understanding of these causes. All that is needed is a strategy for avoiding them.

The key to avoiding memory consistency errors is understanding the  _happens-before_  relationship. This relationship is simply a guarantee that memory writes by one specific statement are visible to another specific statement.
We've already seen two actions that create happens-before relationships.

-   When a statement invokes  `Thread.start`, every statement that has a happens-before relationship with that statement also has a happens-before relationship with every statement executed by the new thread. The effects of the code that led up to the creation of the new thread are visible to the new thread.
-   When a thread terminates and causes a  `Thread.join`  in another thread to return, then all the statements executed by the terminated thread have a happens-before relationship with all the statements following the successful join. The effects of the code in the thread are now visible to the thread that performed the join.

For a list of actions that create happens-before relationships, refer to the  [Summary page of the  `java.util.concurrent`  package.](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/package-summary.html#MemoryVisibility).
# Synchronized Methods
The Java programming language provides two basic synchronization idioms: **_synchronized methods_** and **_synchronized statements_**.
If  `count`  is an instance of  `SynchronizedCounter`, then making these methods synchronized has two effects:

-   First, it is not possible for two invocations of synchronized methods on the same object to interleave. When one thread is executing a synchronized method for an object, all other threads that invoke synchronized methods for the same object block (suspend execution) until the first thread is done with the object.
-   Second, when a synchronized method exits, it automatically establishes a happens-before relationship with  _any subsequent invocation_  of a synchronized method for the same object. This guarantees that changes to the state of the object are visible to all threads.

Note that constructors cannot be synchronized — using the  `synchronized`  keyword with a constructor is a syntax error. Synchronizing constructors doesn't make sense, because only the thread that creates an object should have access to it while it is being constructed.
## Intrinsic Locks and Synchronization
Synchronization is built around an internal entity known as the  _intrinsic lock_  or  _monitor lock_. (The API specification often refers to this entity simply as a "monitor.") Intrinsic locks play a role in both aspects of synchronization: enforcing exclusive access to an object's state and establishing happens-before relationships that are essential to visibility.

Every object has an intrinsic lock associated with it. By convention, a thread that needs exclusive and consistent access to an object's fields has to  _acquire_  the object's intrinsic lock before accessing them, and then  _release_  the intrinsic lock when it's done with them. A thread is said to  _own_  the intrinsic lock between the time it has acquired the lock and released the lock. As long as a thread owns an intrinsic lock, no other thread can acquire the same lock. The other thread will block when it attempts to acquire the lock.

When a thread releases an intrinsic lock, a happens-before relationship is established between that action and any subsequent acquisition of the same lock.
## Synchronized Statements
Another way to create synchronized code is with  _synchronized statements_. Unlike synchronized methods, synchronized statements must specify the object that provides the intrinsic lock:

    public void addName(String name) {
        synchronized(this) {
            lastName = name;
            nameCount++;
        }
        nameList.add(name);
    }
括号里的this有什么用？？？？
In this example, the `addName` method needs to synchronize changes to `lastName` and `nameCount`, but also needs to avoid synchronizing invocations of other objects' methods. (Invoking other objects' methods from synchronized code can create problems that are described in the section on [Liveness](https://docs.oracle.com/javase/tutorial/essential/concurrency/liveness.html).) Without synchronized statements, there would have to be a separate, unsynchronized method for the sole purpose of invoking `nameList.add`.
### Reentrant Synchronization
Recall that a thread cannot acquire a lock owned by another thread. But a thread _can_ acquire a lock that it already owns. Allowing a thread to acquire the same lock more than once enables _reentrant synchronization_. This describes a situation where synchronized code, directly or indirectly, invokes a method that also contains synchronized code, and both sets of code use the same lock. Without reentrant synchronization, synchronized code would have to take many additional precautions to avoid having a thread cause itself to block.
### Atomic Access
In programming, an _atomic_ action is one that effectively happens all at once. An atomic action cannot stop in the middle: it either happens completely, or it doesn't happen at all. No side effects of an atomic action are visible until the action is complete.
there are actions you can specify that are atomic:

-   Reads and writes are atomic for reference variables and for most primitive variables (all types except  `long`  and  `double`).
-   Reads and writes are atomic for  _all_  variables declared  `volatile`  (_including_  `long`  and  `double`  variables).

Atomic actions cannot be interleaved, so they can be used without fear of thread interference. However, this does not eliminate all need to synchronize atomic actions, because memory consistency errors are still possible. Using  `volatile`  variables reduces the risk of memory consistency errors, because any write to a  `volatile`  variable establishes a happens-before relationship with subsequent reads of that same variable. This means that changes to a  `volatile`  variable are always visible to other threads. What's more, it also means that when a thread reads a  `volatile`  variable, it sees not just the latest change to the  `volatile`, but also the side effects of the code that led up the change.

Using simple atomic variable access is more efficient than accessing these variables through synchronized code, but requires more care by the programmer to avoid memory consistency errors. Whether the extra effort is worthwhile depends on the size and complexity of the application.
## Liveness
A concurrent application's ability to execute in a timely manner is known as its _liveness_. This section describes the most common kind of liveness problem, [deadlock](https://docs.oracle.com/javase/tutorial/essential/concurrency/deadlock.html), and goes on to briefly describe two other liveness problems, [starvation and livelock](https://docs.oracle.com/javase/tutorial/essential/concurrency/starvelive.html).
## Deadlock
_Deadlock_ describes a situation where two or more threads are blocked forever, waiting for each other。
## Starvation
_Starvation_ describes a situation where a thread is unable to gain regular access to shared resources and is unable to make progress. This happens when shared resources are made unavailable for long periods by "greedy" threads.
## Livelock
A thread often acts in response to the action of another thread. If the other thread's action is also a response to the action of another thread, then _livelock_ may result. As with deadlock, livelocked threads are unable to make further progress. However, the threads are not blocked — they are simply too busy responding to each other to resume work.
## Guarded Blocks
Threads often have to coordinate their actions. The most common coordination idiom is the _guarded block_. Such a block begins by polling a condition that must be true before the block can proceed.
[看代码](https://docs.oracle.com/javase/tutorial/essential/concurrency/guardmeth.html)
## Immutable Objects
An object is considered  _immutable_  if its state cannot change after it is constructed. Maximum reliance on immutable objects is widely accepted as a sound strategy for creating simple, reliable code.

Immutable objects are particularly useful in concurrent applications. Since they cannot change state, they cannot be corrupted by thread interference or observed in an inconsistent state.
### A Strategy for Defining Immutable Objects
The following rules define a simple strategy for creating immutable objects. Not all classes documented as "immutable" follow these rules. This does not necessarily mean the creators of these classes were sloppy — they may have good reason for believing that instances of their classes never change after construction. However, such strategies require sophisticated analysis and are not for beginners.

1.  Don't provide "setter" methods — methods that modify fields or objects referred to by fields.
2.  Make all fields  `final`  and  `private`.
3.  Don't allow subclasses to override methods. The simplest way to do this is to declare the class as  `final`. A more sophisticated approach is to make the constructor  `private`  and construct instances in factory methods.
4.  If the instance fields include references to mutable objects, don't allow those objects to be changed:
    -   Don't provide methods that modify the mutable objects.
    -   Don't share references to the mutable objects. Never store references to external, mutable objects passed to the constructor; if necessary, create copies, and store references to the copies. Similarly, create copies of your internal mutable objects when necessary to avoid returning the originals in your methods.

[代码](https://docs.oracle.com/javase/tutorial/essential/concurrency/imstrat.html)
## High Level Concurrency Objects
So far, this lesson has focused on the low-level APIs that have been part of the Java platform from the very beginning. These APIs are adequate for very basic tasks, but higher-level building blocks are needed for more advanced tasks. This is especially true for massively concurrent applications that fully exploit today's multiprocessor and multi-core systems.
### Lock Objects
Synchronized code relies on a simple kind of reentrant lock. This kind of lock is easy to use, but has many limitations. More sophisticated locking idioms are supported by the  [`java.util.concurrent.locks`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/locks/package-summary.html)  package. We won't examine this package in detail, but instead will focus on its most basic interface,  [`Lock`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/locks/Lock.html).

`Lock`  objects work very much like the implicit locks used by synchronized code. As with implicit locks, only one thread can own a  `Lock`  object at a time.  `Lock`  objects also support a  `wait/notify`  mechanism, through their associated  [`Condition`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/locks/Condition.html)  objects.

The biggest advantage of  `Lock`  objects over implicit locks is their ability to back out of an attempt to acquire a lock. The  `tryLock`  method backs out if the lock is not available immediately or before a timeout expires (if specified). The  `lockInterruptibly`  method backs out if another thread sends an interrupt before the lock is acquired.
[代码](https://docs.oracle.com/javase/tutorial/essential/concurrency/newlocks.html)
## Executors
In all of the previous examples, there's a close connection between the task being done by a new thread, as defined by its `Runnable` object, and the thread itself, as defined by a `Thread` object. This works well for small applications, but in large-scale applications, it makes sense to separate thread management and creation from the rest of the application. Objects that encapsulate these functions are known as _executors_.The following subsections describe executors in detail.
-   [Executor Interfaces](https://docs.oracle.com/javase/tutorial/essential/concurrency/exinter.html)  define the three executor object types.
-   [Thread Pools](https://docs.oracle.com/javase/tutorial/essential/concurrency/pools.html)  are the most common kind of executor implementation.
-   [Fork/Join](https://docs.oracle.com/javase/tutorial/essential/concurrency/forkjoin.html)  is a framework (new in JDK 7) for taking advantage of multiple processors.
## Executor Interfaces
The  `java.util.concurrent`  package defines three executor interfaces:

-   `Executor`, a simple interface that supports launching new tasks.
-   `ExecutorService`, a subinterface of  `Executor`, which adds features that help manage the lifecycle, both of the individual tasks and of the executor itself.
-   `ScheduledExecutorService`, a subinterface of  `ExecutorService`, supports future and/or periodic execution of tasks.

Typically, variables that refer to executor objects are declared as one of these three interface types, not with an executor class type.
## The  `Executor`  Interface
The  [`Executor`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Executor.html)  interface provides a single method,  `execute`, designed to be a drop-in replacement for a common thread-creation idiom. If  `r`  is a  `Runnable`  object, and  `e`  is an  `Executor`  object you can replace

(new Thread(r)).start();

with

e.execute(r);

However, the definition of  `execute`  is less specific. The low-level idiom creates a new thread and launches it immediately. Depending on the  `Executor`  implementation,  `execute`  may do the same thing, but is more likely to use an existing worker thread to run  `r`, or to place  `r`  in a queue to wait for a worker thread to become available. (We'll describe worker threads in the section on  [Thread Pools](https://docs.oracle.com/javase/tutorial/essential/concurrency/pools.html).)

The executor implementations in  `java.util.concurrent`  are designed to make full use of the more advanced  `ExecutorService`  and  `ScheduledExecutorService`  interfaces, although they also work with the base  `Executor`  interface.

## The  `ExecutorService`  Interface

The  [`ExecutorService`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ExecutorService.html)  interface supplements  `execute`  with a similar, but more versatile  `submit`  method. Like  `execute`,  `submit`  accepts  `Runnable`  objects, but also accepts  [`Callable`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Callable.html)  objects, which allow the task to return a value. The  `submit`  method returns a  [`Future`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Future.html)  object, which is used to retrieve the  `Callable`  return value and to manage the status of both  `Callable`  and  `Runnable`  tasks.

`ExecutorService`  also provides methods for submitting large collections of  `Callable`  objects. Finally,  `ExecutorService`  provides a number of methods for managing the shutdown of the executor. To support immediate shutdown, tasks should handle  [interrupts](https://docs.oracle.com/javase/tutorial/essential/concurrency/interrupt.html)  correctly.

## The  `ScheduledExecutorService`  Interface

The  [`ScheduledExecutorService`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ScheduledExecutorService.html)  interface supplements the methods of its parent  `ExecutorService`  with  `schedule`, which executes a  `Runnable`  or  `Callable`  task after a specified delay. In addition, the interface defines  `scheduleAtFixedRate`  and  `scheduleWithFixedDelay`, which executes specified tasks repeatedly, at defined intervals.
# Thread Pools

Most of the executor implementations in  `java.util.concurrent`  use  _thread pools_, which consist of  _worker threads_. This kind of thread exists separately from the  `Runnable`  and  `Callable`  tasks it executes and is often used to execute multiple tasks.

Using worker threads minimizes the overhead due to thread creation. Thread objects use a significant amount of memory, and in a large-scale application, allocating and deallocating many thread objects creates a significant memory management overhead.

One common type of thread pool is the  _fixed thread pool_. This type of pool always has a specified number of threads running; if a thread is somehow terminated while it is still in use, it is automatically replaced with a new thread. Tasks are submitted to the pool via an internal queue, which holds extra tasks whenever there are more active tasks than threads.

An important advantage of the fixed thread pool is that applications using it  _degrade gracefully_. 

A simple way to create an executor that uses a fixed thread pool is to invoke the  [`newFixedThreadPool`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Executors.html#newFixedThreadPool-int-)  factory method in  [`java.util.concurrent.Executors`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Executors.html)  This class also provides the following factory methods:

-   The  [`newCachedThreadPool`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Executors.html#newCachedThreadPool-int-)  method creates an executor with an expandable thread pool. This executor is suitable for applications that launch many short-lived tasks.
-   The  [`newSingleThreadExecutor`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Executors.html#newSingleThreadExecutor-int-)  method creates an executor that executes a single task at a time.
-   Several factory methods are  `ScheduledExecutorService`  versions of the above executors.

If none of the executors provided by the above factory methods meet your needs, constructing instances of  [`java.util.concurrent.ThreadPoolExecutor`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ThreadPoolExecutor.html)  or  [`java.util.concurrent.ScheduledThreadPoolExecutor`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ScheduledThreadPoolExecutor.html)  will give you additional options.
### Fork/Join

The fork/join framework is an implementation of the  `ExecutorService`  interface that helps you take advantage of multiple processors. It is designed for work that can be broken into smaller pieces recursively. The goal is to use all the available processing power to enhance the performance of your application.

As with any  `ExecutorService`  implementation, the fork/join framework distributes tasks to worker threads in a thread pool. The fork/join framework is distinct because it uses a  _work-stealing_  algorithm. Worker threads that run out of things to do can steal tasks from other threads that are still busy.

The center of the fork/join framework is the  [`ForkJoinPool`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ForkJoinPool.html)  class, an extension of the  `AbstractExecutorService`  class.  `ForkJoinPool`  implements the core work-stealing algorithm and can execute  [`ForkJoinTask`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ForkJoinTask.html)  processes.
# Concurrent Collections

The  `java.util.concurrent`  package includes a number of additions to the Java Collections Framework. These are most easily categorized by the collection interfaces provided:

-   [`BlockingQueue`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/BlockingQueue.html)  defines a first-in-first-out data structure that blocks or times out when you attempt to add to a full queue, or retrieve from an empty queue.
-   [`ConcurrentMap`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentMap.html)  is a subinterface of  [`java.util.Map`](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html)  that defines useful atomic operations. These operations remove or replace a key-value pair only if the key is present, or add a key-value pair only if the key is absent. Making these operations atomic helps avoid synchronization. The standard general-purpose implementation of  `ConcurrentMap`  is  [`ConcurrentHashMap`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentHashMap.html), which is a concurrent analog of  [`HashMap`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html).
-   [`ConcurrentNavigableMap`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentNavigableMap.html)  is a subinterface of  `ConcurrentMap`  that supports approximate matches. The standard general-purpose implementation of  `ConcurrentNavigableMap`  is  [`ConcurrentSkipListMap`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentSkipListMap.html), which is a concurrent analog of  [`TreeMap`](https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html).

All of these collections help avoid  [Memory Consistency Errors](https://docs.oracle.com/javase/tutorial/essential/concurrency/memconsist.html)  by defining a happens-before relationship between an operation that adds an object to the collection with subsequent operations that access or remove that object.
### Atomic Variables

The  [`java.util.concurrent.atomic`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/atomic/package-summary.html)  package defines classes that support atomic operations on single variables. All classes have  `get`  and  `set`  methods that work like reads and writes on  `volatile`  variables. That is, a  `set`  has a happens-before relationship with any subsequent  `get`  on the same variable. The atomic  `compareAndSet`  method also has these memory consistency features, as do the simple atomic arithmetic methods that apply to integer atomic variables.
### Concurrent Random Numbers

In JDK 7,  [`java.util.concurrent`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/package-summary.html)  includes a convenience class,  [`ThreadLocalRandom`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ThreadLocalRandom.html), for applications that expect to use random numbers from multiple threads or  `ForkJoinTask`s.

For concurrent access, using  `ThreadLocalRandom`  instead of  `Math.random()`  results in less contention and, ultimately, better performance.

All you need to do is call  `ThreadLocalRandom.current()`, then call one of its methods to retrieve a random number. Here is one example:

    int r = ThreadLocalRandom.current() .nextInt(4, 77);
### For Further Reading

-   _Concurrent Programming in Java: Design Principles and Pattern (2nd Edition)_  by Doug Lea. A comprehensive work by a leading expert, who's also the architect of the Java platform's concurrency framework.
-   _Java Concurrency in Practice_  by Brian Goetz, Tim Peierls, Joshua Bloch, Joseph Bowbeer, David Holmes, and Doug Lea. A practical guide designed to be accessible to the novice.
-   _Effective Java Programming Language Guide (2nd Edition)_  by Joshua Bloch. Though this is a general programming guide, its chapter on threads contains essential "best practices" for concurrent programming.
-   _Concurrency: State Models & Java Programs (2nd Edition)_, by Jeff Magee and Jeff Kramer. An introduction to concurrent programming through a combination of modeling and practical examples.
-   _[Java Concurrent Animated](http://sourceforge.net/projects/javaconcurrenta/):_  Animations that show usage of concurrency features.
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
