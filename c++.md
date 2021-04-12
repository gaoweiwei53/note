# 基础知识

在C++中，有两种简单的定义常量的方式
- 使用`#define`预处理器
- 使用`const`关键字

## C++存储类
存储类定义 C++ 程序中变量/函数的范围（可见性）和生命周期。
- static
    - 修饰局部变量时，可使，可使其在程序的生命周期内保持存在且保持变量的值不变
    - 修饰全局变量时会使变量的作用域限制在声明它的文件内
    - 修饰类数据成员时，会使仅有一个该成员的副本被类的所有对象共享。
- extern： 修饰一个全局变量，该全局变量对所有的程序文件都是可见的
- mutable

## 函数
当调用函数时，有三种向函数传递参数的方式：
- 传值调用
- 指针调用
- 引用调用

默认情况下，C++ 使用传值调用来传递参数
## 指针
指针和引用的区别：
- 不存在空引用。引用必须连接到一块合法的内存。
- 一旦引用被初始化为一个对象，就不能被指向到另一个对象。指针可以在任何时候指向到另一个对象。
- 引用必须在创建时被初始化。指针可以在任何时间被初始化。
> 可以理解为，指针是指向某个变量内存地址的变量，而引用是该变量的别名
> 
## 引用
两个重要的概念：
- 把引用作为参数
- 把引用作为返回值
```c++
// 把引用作为参数
void swap(int& x, int& y)
{
   int temp;
   temp = x; /* 保存地址 x 的值 */
   x = y;    /* 把 y 赋值给 x */
   y = temp; /* 把 x 赋值给 y  */
  
   return;
}

// 把引用作为返回值
double vals[] = {10.1, 12.6, 33.1, 24.1, 50.0};
 
double& setValues(int i) {  
   double& ref = vals[i];    
   return ref;   // 返回第 i 个元素的引用，ref 是一个引用变量，ref 引用 vals[i]
}
```
当返回一个引用时，要注意被引用的对象不能超出作用域。所以返回一个对局部变量的引用是不合法的

# 面向对象
## 类和对象
```c++
class Box
{
   public:
      double length;      // 长度
      double breadth;     // 宽度
      double height;      // 高度
   
      double getVolume(void)
      {
         return length * breadth * height;
      }
};
```
成员函数可以定义在类定义内部，或者单独使用**范围解析运算符** `::`来定义。在类定义中定义的成员函数把函数声明为**内联**的，即便没有使用 `inline` 标识符。

也可以在类的外部使用范围解析运算符 :: 定义该函数
```c++
double Box::getVolume(void)
{
    return length * breadth * height;
}
```
## C++访问修饰符
C++类中默认是private类型的，继承默认也是。

有public, protected, private三种继承方式，它们相应地改变了基类成员的访问属性。
- public 继承：基类 public 成员，protected 成员，private 成员的访问属性在派生类中分别变成：public, protected, private
- protected 继承：基类 public 成员，protected 成员，private 成员的访问属性在派生类中分别变成：protected, protected, private
- private 继承：基类 public 成员，protected 成员，private 成员的访问属性在派生类中分别变成：private, private, private

## 类的构造函数和析构函数
### 构造函数
#### 使用初始化列表来初始化字段
```c++
Line::Line( double len): length(len)
{
    cout << "Object is being created, length = " << len << endl;
}

// 相当于
Line::Line( double len)
{
    length = len;
    cout << "Object is being created, length = " << len << endl;
}

// 假设有一个类 C，具有多个字段 X、Y、Z 等需要进行初始化
C::C( double a, double b, double c): X(a), Y(b), Z(c)
{
  ....
}
```
### 析构函数
### 拷贝构造函数
## 友元函数
类的友元函数是定义在类外部，但有权访问类的所有私有（private）成员和保护（protected）成员。尽管友元函数的原型有在类的定义中出现过，但是友元函数并不是成员函数。

友元可以是一个函数，该函数被称为友元函数；友元也可以是一个类，该类被称为友元类，在这种情况下，整个类及其所有成员都是友元。

如果要声明函数为一个类的友元，需要在类定义中该函数原型前使用关键字 **friend**
```c++
class Box
{
   double width;
public:
   double length;
   friend void printWidth( Box box );
   void setWidth( double wid );
};
```
## 内联函数

## 运算符重载
```c++
Box operator+(const Box&);
```
## 多态
C++ 多态意味着调用成员函数时，会根据调用函数的对象的类型来执行不同的函数。
### 虚函数
虚函数 是在基类中使用关键字 **virtual** 声明的函数。在派生类中重新定义基类中定义的虚函数时，会告诉编译器不要静态链接到该函数。
### 纯虚函数
您可能想要在基类中定义虚函数，以便在派生类中重新定义该函数更好地适用于对象，但是您在基类中又不能对虚函数给出有意义的实现，这个时候就会用到纯虚函数。
```c++
class Shape {
   protected:
      int width, height;
   public:
      Shape( int a=0, int b=0)
      {
         width = a;
         height = b;
      }
      // pure virtual function
      virtual int area() = 0;
};
```
= 0 告诉编译器，函数没有主体，上面的虚函数是纯虚函数。

## 接口(抽象类)
如果类中至少有一个函数被声明为纯虚函数，则这个类就是抽象类。纯虚函数是通过在声明中使用 "= 0" 来指定的。
```c++
class Box
{
   public:
      // 纯虚函数
      virtual double getVolume() = 0;
   private:
      double length;      // 长度
      double breadth;     // 宽度
      double height;      // 高度
};
```
