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
```
