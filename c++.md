# 基础知识

在C++中，有两种简单的定义常量的方式
- 使用`#define`预处理器
- 使用`const`关键字

## keywords
- static
    - 修饰局部变量时，可使，可使其在程序的生命周期内保持存在且保持变量的值不变
    - 修饰全局变量时会使变量的作用域限制在声明它的文件内
    - 修饰类数据成员时，会使仅有一个该成员的副本被类的所有对象共享。
- extern： 修饰一个全局变量，该全局变量对所有的程序文件都是可见的
- auto: 
# 变量和类型
C+初始化方式
现代C++建议使用列表初始化，C++11提出
```c++
int a{}, b{}, c{2};
```
## auto 关键词
C++11引入了auto关键字，可用来推断类型

`const`定义变量时必须初始化

`sizeof`可以查看变量所占的内存

## 逗号运算符
逗号运算符的优先级低于赋值运算符，所以一起使用的时候要用括号、
```c++
auto a{4}, b{3};
auto d = (a, b);
```
## 函数
当调用函数时，有三种向函数传递参数的方式：
- 传值调用
- 指针调用
- 引用调用

默认情况下，C++ 使用传值调用来传递参数

### const与形参
```c++
void f(cosnt int x, const int y);
void g(const int *p, const int n);
void h(int * const q, const int n);
void h(const int * const s, const int n);
```
### 可变形参
```c++
#include<iostream>
double average(std::initializer_list<double> scores){
    auto n{0};
    double all{0};
    for (auto score: scores){
        all += score;
        n++;
    }
    if(n > 0) return all /= n;
    return 0;
}
```
> std::initializer_list<>里的是const对象，不可修改，且类型一样

### incline函数
编译器在编译时将函数调用语句替换为函数体的代码并对函数体的局部变量名做一些调整。免去函数调用时传递参数和得到返回结果的开销。
```c++
inline int add(const int x, const int y){
    return x + y;
}
int main(){
    add(3, 4);
}
```
## 指针
指针和引用的区别：
- 不存在空引用。引用必须连接到一块合法的内存。
- 一旦引用被初始化为一个对象，就不能被指向到另一个对象。指针可以在任何时候指向到另一个对象。
- 引用必须在创建时被初始化。指针可以在任何时间被初始化。
> 可以理解为，指针是指向某个变量内存地址的变量，而引用是该变量的别名

初始化一个空指针：                                                                                                                           
```c++
int *p2{0};
int *p1{nullptr}; // c++11后推荐使用
```

### `const`和指针
```c++
// 从右向左看
int i{0};
int * const p = &i; // 指针变量p本身本身不可变，其指向的内容可变
const int *q = &i;  // 指针变量q指向的内容不可变，其本身可变
int const *s = &i;  // 和2同样的意思
const int * const ptr = &i; // 都不可变
```

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
C++类中的成员默认是private类型的，继承默认也是。struct里的成员默认时public类型。

有public, protected, private三种继承方式，它们相应地改变了基类成员的访问属性。
- public 继承：基类 public 成员，protected 成员，private 成员的访问属性在派生类中分别变成：public, protected, private
- protected 继承：基类 public 成员，protected 成员，private 成员的访问属性在派生类中分别变成：protected, protected, private
- private 继承：基类 public 成员，protected 成员，private 成员的访问属性在派生类中分别变成：private, private, private
## this指针
类的成员函数都会被编译器转换为一个普通的全局函数，这个普通的函数包含一个特殊的叫做this的指针形参，这个形参就指向调用这个函数的那个对象，即存储这个调用对象的地址。

每个对象的数据成员都有自己单独的内存，但类的成员函数为类的所有对象共享。编译器将类的成员函数转换为普通的外部函数，其中包含了一个指针变量，指向调用这个函数的类对象，通过类对象调用成员函数，就是将这个对象的指针传递给这个函数。即成员函数的代码只占据一块内存，是所有对象共享的，而每个对象的数据成员要占据独立的内存。
## 类的构造函数和析构函数
### 构造函数
一但定义了自己的构造函数，编译器就不再生成默认的构造函数，可以自己添加：
```c++
Date(){}
Date() = default; // 使用default关键字
```
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
### [拷贝构造函数](https://www.geeksforgeeks.org/copy-constructor-in-cpp/)
拷贝构造函数使用引用作为参数初始化创建中的对象。

When is a user-defined copy constructor needed? 
If we don’t define our own copy constructor, the C++ compiler creates a default copy constructor for each class which does a member-wise copy between objects. The compiler created copy constructor works fine in general. We need to define our own copy constructor only if an object has pointers or any runtime allocation of the resource like filehandle, a network connection..etc.

类的对象需要拷贝时，拷贝构造函数将会被调用。以下情况都会调用拷贝构造函数：

1) 一个对象以值传递的方式传入函数体
2) 一个对象以值传递的方式从函数返回
3) 一个对象需要通过另外一个对象进行初始化。

当对象含有指针数据成员，并用它初始化同类型的另一个对象时，默认的拷贝构造函数只能将该对象的数据成员复制给另一个对象，而不能将该对象中指针所指向的内存单元也复制过去，这样就可能出现同一内存单元释放两次，导致程序运行出错。
拷贝构造函数的形式：
```c++
classname (const classname &obj) {
   // 构造函数的主体
}
```
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
