# rust简介
# 工具
1. 安装、更新、卸载rustup
```bash
$ curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh

$ rustup update

$ rustup self uninstall

$ rustc --version
```
2. Cargo
```bash
$ cargo --version

# 新建项目
$ cargo new hello_cargo

$ cargo build

# 编译代码直接运行
$ cargo run

# 检查代码是否可编译
$ cargo check

# 当项目可发布的时候可用
$ cargo build --release
```
# 基本语法
## 变量
默认变量是不可变的，可避免变量被意外修改而导致程序出现bug的问题。使用`mut`关键字可声明可变的变量。

> java中没有`const`关键字，常使用 `final static`定义常量。C++中没有`final`关键字

### 数据类型
Rust是一种静态类型语言，意味着在编译期rust必须知道所有变量的类型。
#### 标量类型
Rust有四种标量类型(Scalar type)的数据：
- integers
  - `i8` ~ `i128`, `u8` ~ `u128`, `isize`, `usize` ， 默认为`i32`
- floating-point numbers
  - `f32`
  - `f64`, 默认
- Booleans
  -`bool`
- characters

### 复合类型
Rust有两种基本复合类型：tuples和arrays
#### tuple
固定长度，长度不可变。

```rust
fn main() {
    let tup: (i32, f64, u8) = (500, 6.4, 1);
```

想要获取里面的元素，可使用模式匹配来分解(destructing) tuple
```rust
fn main() {
    let tup = (500, 6.4, 1);

    let (x, y, z) = tup;

    println!("The value of y is: {}", y);
}
```
也可以直接使用`.`直接获取
```rust
fn main() {
    let x: (i32, f64, u8) = (500, 6.4, 1);

    let five_hundred = x.0;

    let six_point_four = x.1;

    let one = x.2;
}
```

#### array
与tuple不同的是，array里面的元素类型必须相同

定义与初始化：
```rust
fn main() {
    let a = [1, 2, 3, 4, 5];
    
    // 指出类型和元素数量
    let a: [i32; 5] = [1, 2, 3, 4, 5];
    
    // 如果想将array的所有元素初始化为相同的内容
    // array为5个3
    let a = [3; 5];
}
```
获取数组元素
```rust
fn main() {
    let a = [1, 2, 3, 4, 5];

    let first = a[0];
    let second = a[1];
}
```
## 函数
rust使用`fn`定义函数，rust中函数名称一般使用小写，多个字母组合用`_`连接：
```rust
fn main() {
    println!("Hello, world!");

    another_function();
}

fn another_function() {
    println!("Another function.");
}
```
> rust函数的形参必须声明类型

Expression也是statement的一部分，下面的代码中`{}`就是一个Expression，`let y = {...};`是一个statement
```rust
fn main() {
    let x = 5;

    let y = {
        let x = 3;
        x + 1     // 注意这里没有分号结尾，意思是作为返回值
    };

    println!("The value of y is: {}", y);
}
```
### 带返回值的函数
rust使用`->`指示函数会返回值
```rust
fn five() -> i32 {
    5
}

fn main() {
    let x = five();

    println!("The value of x is: {}", x);
}
```
rust中函数的返回值一般就是block中最后一行表达式，也可以使用`return`提前返回值

## 流程控制
### if语句
```rust
fn main() {
    let number = 3;

    if number < 5 {
        println!("condition was true");
    } else {
        println!("condition was false");
    }
}
```
> if 后的condition中的变量必须是布尔类型

可以在let声明中使用if表达式
```rust
fn main() {
    let condition = true;
    
    // 两种情况的返回值类型必须相同
    let number = if condition { 5 } else { 6 };

    println!("The value of number is: {}", number);
}
```
### 循环
Rust有三种循环表达式: `loop`, `while`, `for`

#### loop
`loop`会不断重复执行语句块，可用`break`停止

从Loop表达式中返回值, 返回值写在`break后面`：
```rust
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            break counter * 2;
        }
    };

    println!("The result is {}", result);
}
```
#### while
```rust
fn main() {
    let mut number = 3;

    while number != 0 {
        println!("{}!", number);

        number -= 1;
    }

    println!("LIFTOFF!!!");
}
```

#### for
用for遍历集合：
```rust
fn main() {
    let a = [10, 20, 30, 40, 50];

    for element in a.iter() {
        println!("the value is: {}", element);
    }
}
```

```rust
fn main() {
    for number in (1..4).rev() {
        println!("{}!", number);
    }
    println!("LIFTOFF!!!");
}
```
# Ownership
所有的程序必须要管理所使用的内存。有的语言使用垃圾回收器，有的语言让程序员手动管理。Rust使用的是另一种方法：**ownership**
```rust
fn main() {
    {
        let s = String::from("hello"); // s is valid from this point forward

        // do stuff with s
    }                                  // this scope is now over, and s is no
                                       // longer valid
}
```
当一个变量离开作用域就会释放内存。在离开`}`时，Rust自动调用`drop`函数释放内存
```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1; // 浅拷贝

    println!("{}, world!", s1); // error 
}
```
变量`s1`会不可用，避免两次释放同一块内存的问题
```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1.clone();

    println!("s1 = {}, s2 = {}", s1, s2);
}
```
当想使用深拷贝时，调用`clone()`方法

```rust
fn main() {
    let s = String::from("hello");  // s comes into scope

    takes_ownership(s);             // s's value moves into the function...
                                    // ... and so is no longer valid here

    let x = 5;                      // x comes into scope

    makes_copy(x);                  // x would move into the function,
                                    // but i32 is Copy, so it's okay to still
                                    // use x afterward

} // Here, x goes out of scope, then s. But because s's value was moved, nothing
  // special happens.

fn takes_ownership(some_string: String) { // some_string comes into scope
    println!("{}", some_string);
} // Here, some_string goes out of scope and `drop` is called. The backing
  // memory is freed.

fn makes_copy(some_integer: i32) { // some_integer comes into scope
    println!("{}", some_integer);
} // Here, some_integer goes out of scope. Nothing special happens.
```
main()函数中变量s在传入函数后就不在可用了，x这种基本类型的变量还可用，因为是复制了一份。
## 返回值和作用域
返回一个值同时会转换ownership:
```rust
fn main() {
    let s1 = gives_ownership();         // gives_ownership moves its return
                                        // value into s1

    let s2 = String::from("hello");     // s2 comes into scope

    let s3 = takes_and_gives_back(s2);  // s2 is moved into
                                        // takes_and_gives_back, which also
                                        // moves its return value into s3
} // Here, s3 goes out of scope and is dropped. s2 goes out of scope but was
  // moved, so nothing happens. s1 goes out of scope and is dropped.

fn gives_ownership() -> String {             // gives_ownership will move its
                                             // return value into the function
                                             // that calls it

    let some_string = String::from("hello"); // some_string comes into scope

    some_string                              // some_string is returned and
                                             // moves out to the calling
                                             // function
}

// takes_and_gives_back will take a String and return one
fn takes_and_gives_back(a_string: String) -> String { // a_string comes into
                                                      // scope

    a_string  // a_string is returned and moves out to the calling function
}
```
## 引用和borrowing
```rust
fn main() {
    let s1 = String::from("hello");

    let len = calculate_length(&s1);

    println!("The length of '{}' is {}.", s1, len);
}

fn calculate_length(s: &String) -> usize {
    s.len()
}
```
方法`calculate_length(&s1)`传入的是变量s1的引用，不会把s1的ownership传给调用它的方法，所有后面s1还可用。
> 注: rust中禁止在函数中修改引用的值

### 可变引用
如果想可以引用的值我们可以在形参中加`mut`:
```rust
fn main() {
    let mut s = String::from("hello");

    change(&mut s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```
但是一个值同时**只能有一个可变引用**，并且**不能同时有不可变引用和可变引用**，但是可以同时有多个不可变引用.
> 引用的作用域是从定义引用开始，到引用不再使用结束。
所以如果某个值后面不在使用，那么允许对它再次进行可变引用，如下：
```rust
fn main() {
    let mut s = String::from("hello");

    let r1 = &s; // no problem
    let r2 = &s; // no problem
    println!("{} and {}", r1, r2);
    // r1 and r2 are no longer used after this point

    let r3 = &mut s; // no problem
    println!("{}", r3);
}
```
### Dangling References
*dangling pointer*指的是通过释放之前被使用内存后指向该内存的的指针。Rust可以保证不会有`Dangling References`
```rust
fn main() {
    let reference_to_nothing = dangle();
}

fn dangle() -> &String {
    let s = String::from("hello");

    &s // 错，s在作用域结束后会释放该内存，不能再返回对它的引用
    s  // 可，通过转换它的ownership
}
```
### The Slice Type
slice没有ownership
#### String Slices
一个* string slice*就是对string的部分引用，如下:
```rust
fn main() {
    let s = String::from("hello world");

    let hello = &s[0..5];
    let world = &s[6..11];
}
```
```rust

#![allow(unused)]
fn main() {
let s = String::from("hello");

let slice = &s[0..2];
let slice = &s[..2]; // 0 可省略

let len = s.len();

let slice = &s[3..len];
let slice = &s[3..]; // len可省略
}

```
重写返回第一个单词的函数：
```rust
fn first_word(s: &String) -> &str {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }

    &s[..]
}

fn main() {}
```
使用slice可让一些错误提前可见，如下：
```rust
fn first_word(s: &String) -> &str {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }

    &s[..]
}

fn main() {
    let mut s = String::from("hello world");

    let word = first_word(&s);

    s.clear(); // error! 错误

    println!("the first word is: {}", word);
}
```
因为s已经有了一个不可变引用，`s.clear`需要得到一个可变引用，这在Rust是不被允许的，编译会出错(之前用索引的代码编译时不会报错)，所以能避免安全性错误。

#### 用String Slices作为形参
```rust
// 不是 fn first_word(s: &String) -> &str {
// 形参用 s: &str 代替 s: &String
fn first_word(s: &str) -> &str {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }

    &s[..]
}

fn main() {
    let my_string = String::from("hello world");

    // first_word works on slices of `String`s
    let word = first_word(&my_string[..]);

    let my_string_literal = "hello world";

    // first_word works on slices of string literals
    let word = first_word(&my_string_literal[..]);

    // Because string literals *are* string slices already,
    // this works too, without the slice syntax!
    let word = first_word(my_string_literal);
}
```
这样写程序更加健壮，因为既可以传入string slice也可以传入String

### 总结
ownership, borrowing, slices这些概念让Rust程序能在编译时期就能保证内存安全。

# Struct
定义struct：
```rust
struct User {
    username: String,
    email: String,
    sign_in_count: u64,
    active: bool,
}

fn main() {
    let user1 = User {
        email: String::from("someone@example.com"),
        username: String::from("someusername123"),
        active: true,
        sign_in_count: 1,
    };
    user1.email = String::from("anotheremail@example.com");
}
```
当形参名和结构体里的字段名一样时，可以像如下简写：
```rust
struct User {
    username: String,
    email: String,
    sign_in_count: u64,
    active: bool,
}

fn build_user(email: String, username: String) -> User {
    User {
        email,     // 简写
        username,
        active: true,
        sign_in_count: 1,
    }
}

fn main() {
    let user1 = build_user(
        String::from("someone@example.com"),
        String::from("someusername123"),
    );
}
### 使用其他结构体字段
```rust
struct User {
    username: String,
    email: String,
    sign_in_count: u64,
    active: bool,
}

fn main() {
    let user1 = User {
        email: String::from("someone@example.com"),
        username: String::from("someusername123"),
        active: true,
        sign_in_count: 1,
    };

    let user2 = User {
        email: String::from("another@example.com"),
        username: String::from("anotherusername567"),
        active: user1.active,
        sign_in_count: user1.sign_in_count,
    };
    
    let user3 = User {
        email: String::from("another@example.com"),
        username: String::from("anotherusername567"),
        ..user1  // 剩下的字段和user1的字段内容一样
    };
}
```
### Tuple Struct
```rust
fn main() {
    struct Color(i32, i32, i32);
    struct Point(i32, i32, i32);

    let black = Color(0, 0, 0);
    let origin = Point(0, 0, 0);
}
```
## Struct的具体使用
```rust
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!(
        "The area of the rectangle is {} square pixels.",
        area(&rect1)
    );
}

fn area(rectangle: &Rectangle) -> u32 {
    rectangle.width * rectangle.height
}
```
#### 打印struct
```rust
#[derive(Debug)]  // 加上这一句就可打印
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!("rect1 is {:?}", rect1);
    println!("rect1 is {:#?}", rect1);
}
```
## 结构体里的方法
使用`impl`可定义结构体里的方法。方法的**第一个参数永远是`&self`**
```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }

    fn can_hold(&self, other: &Rectangle) -> bool {
        self.width > other.width && self.height > other.height
    }
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };
    let rect2 = Rectangle {
        width: 10,
        height: 40,
    };
    let rect3 = Rectangle {
        width: 60,
        height: 45,
    };

    println!("Can rect1 hold rect2? {}", rect1.can_hold(&rect2));
    println!("Can rect1 hold rect3? {}", rect1.can_hold(&rect3));
}
```

### Associated Functions
定义与结构体相关的函数，但是没有`self`参数，这种方法叫`associated Function`，它们是函数不是方法。使用时用`::`：
```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {

    // associated Function
    fn square(size: u32) -> Rectangle {
        Rectangle {
            width: size,
            height: size,
        }
    }
}

fn main() {
    let sq = Rectangle::square(3);
}
```
# 枚举 Emnum
## 定义和使用emnum
```rust
fn main() {
    enum IpAddr {
        V4(String),
        V6(String),
    }

    let home = IpAddr::V4(String::from("127.0.0.1"));

    let loopback = IpAddr::V6(String::from("::1"));
}
```
另一个使用枚举的例子：
```rust
enum Message {
    Quit,                       // tuple struct
    Move { x: i32, y: i32 },    // 匿名struct
    Write(String),              // tuple struct
    ChangeColor(i32, i32, i32), // tuple struct
}
fn main() {}
```
上面的在一个enum里定义多个struct等同于直接定义多个struct，如下：
```rust
struct QuitMessage; // unit struct
struct MoveMessage {
    x: i32,
    y: i32,
}
struct WriteMessage(String); // tuple struct
struct ChangeColorMessage(i32, i32, i32); // tuple struct

fn main() {}
```
但是使用enum的代码表示的一个类型，而直接定义的多个struct是不同的的类型。在作为形参的时候，enum类型的形参可以适配所有里面定义的stuct，而直接定义的struct每一种只能适配一种类型。

## 在enum里定义函数
```rust
fn main() {
    enum Message {
        Quit,
        Move { x: i32, y: i32 },
        Write(String),
        ChangeColor(i32, i32, i32),
    }

    impl Message {
        fn call(&self) {
            // method body would be defined here
        }
    }

    let m = Message::Write(String::from("hello"));
    m.call();
}
```
## 一个标准库中的enum `Option`
为了保证程序的健壮性，Rust中没有`null`值这个概念！

Rust使用enum `Option`来表达这个*null*概念：
```rust
#![allow(unused)]
fn main() {
    enum Option<T> {
        Some(T),
        None,
    }
    
    let some_number = Some(5);
    let some_string = Some("a string");
    
    // 使用None必须指明类型
    let absent_number: Option<i32> = None;
}
## `match`控制流操作符
`match`是一个非常强大的控制流操作符。
```rust
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter,
}

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}

fn main() {}
```
`mactch`对应的大括号里的每一行称为*arms*, 每一个*arm*由一个*pattern*和一些代码组成。例如`Coin::Penny`是一个*pattern*，`=>`后的就是对应的代码。代码表达式的值就是返回给整个`match`表达式的值。
```rust
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter,
}

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        // 代码有多行就用大括号
        Coin::Penny => {
            println!("Lucky penny!");
            1
        }
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}

fn main() {}
```
### 绑定到值的pattern
```rsut
#[derive(Debug)]
enum UsState {
    Alabama,
    Alaska,
    // --snip--
}

enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter(UsState),
}

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter(state) => {
            println!("State quarter from {:?}!", state);
            25
        }
    }
}

fn main() {
    value_in_cents(Coin::Quarter(UsState::Alaska));
}

```
### 与`OPtion<T>`匹配
```rust
fn main() {
    fn plus_one(x: Option<i32>) -> Option<i32> {
        match x {
            None => None,
            Some(i) => Some(i + 1),
        }
    }

    let five = Some(5);
    let six = plus_one(five);
    let none = plus_one(None);
}
```
### `_`占位符
`_`会匹配所有情况:
```rust
fn main() {
    let some_u8_value = 0u8;
    match some_u8_value {
        1 => println!("one"),
        3 => println!("three"),
        5 => println!("five"),
        7 => println!("seven"),
        _ => (),
    }
}
```
### `if_let`
有时候如果我们只想关注一种可能性的话，使用`match`表达式会显得冗长，如下：
```rust
fn main() {
    let some_u8_value = Some(0u8);
    match some_u8_value {
        Some(3) => println!("three"),
        _ => (),
    }
}
```
这时候我们可以使用`if let`：
```rust
fn main() {
    let some_u8_value = Some(0u8);
    if let Some(3) = some_u8_value {
        println!("three");
    }
}
```
类似`if else`的结构
```rust
#[derive(Debug)]
enum UsState {
    Alabama,
    Alaska,
    // --snip--
}

enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter(UsState),
}

fn main() {
    let coin = Coin::Penny;
    let mut count = 0;
    match coin {
        Coin::Quarter(state) => println!("State quarter from {:?}!", state),
        _ => count += 1,
    }
}

```
改用`if let`:
```rust
#[derive(Debug)]
enum UsState {
    Alabama,
    Alaska,
    // --snip--
}

enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter(UsState),
}

fn main() {
    let coin = Coin::Penny;
    let mut count = 0;
    if let Coin::Quarter(state) = coin {
        println!("State quarter from {:?}!", state);
    } else {
        count += 1;
    }
}
```
# 泛型
## 函数里的泛型
```rust
fn largest<T>(list: &[T]) -> T {
    let mut largest = list[0];

    for &item in list {
        if item > largest {
            largest = item;
        }
    }

    largest
}

fn main() {
    let number_list = vec![34, 50, 25, 100, 65];

    let result = largest(&number_list);
    println!("The largest number is {}", result);

    let char_list = vec!['y', 'm', 'a', 'q'];

    let result = largest(&char_list);
    println!("The largest char is {}", result);
}
```
> 注：此代码会编译错误

## 结构体泛型
```rust
struct Point<T, U> {
    x: T,
    y: U,
}

fn main() {
    let both_integer = Point { x: 5, y: 10 };
    let both_float = Point { x: 1.0, y: 4.0 };
    let integer_and_float = Point { x: 5, y: 4.0 };
}
```
## 枚举中的泛型
```rust

#![allow(unused)]
fn main() {
enum Option<T> {
    Some(T),
    None,
}
}
```
## 方法里的泛型
```rust
struct Point<T> {
    x: T,
    y: T,
}

impl<T> Point<T> {
    fn x(&self) -> &T {
        &self.x
    }
}

fn main() {
    let p = Point { x: 5, y: 10 };

    println!("p.x = {}", p.x());
}
```
> 注意：在`impl`后也要加`<T>`
混合泛型
```rust
struct Point<T, U> {
    x: T,
    y: U,
}

impl<T, U> Point<T, U> {
    fn mixup<V, W>(self, other: Point<V, W>) -> Point<T, W> {
        Point {
            x: self.x,
            y: other.y,
        }
    }
}

fn main() {
    let p1 = Point { x: 5, y: 10.4 };
    let p2 = Point { x: "Hello", y: 'c' };

    let p3 = p1.mixup(p2);

    println!("p3.x = {}, p3.y = {}", p3.x, p3.y);
}
```
## trait
### 定义和实现trait
```rust
pub trait Summary {
    fn summarize(&self) -> String;
}

pub struct NewsArticle {
    pub headline: String,
    pub location: String,
    pub author: String,
    pub content: String,
}

impl Summary for NewsArticle {
    fn summarize(&self) -> String {
        format!("{}, by {} ({})", self.headline, self.author, self.location)
    }
}

pub struct Tweet {
    pub username: String,
    pub content: String,
    pub reply: bool,
    pub retweet: bool,
}

impl Summary for Tweet {
    fn summarize(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
}


use chapter10::{self, Summary, Tweet};

fn main() {
    let tweet = Tweet {
        username: String::from("horse_ebooks"),
        content: String::from(
            "of course, as you probably already know, people",
        ),
        reply: false,
        retweet: false,
    };

    println!("1 new tweet: {}", tweet.summarize());
}

```
trait定义为`pub`，表示其他的crate可以实现它。

注：当trait和type至少一个为本地的时候，我们才能使用该type来实现此trait
### 默认实现方法
trait可以定义默认的实现方法，实现该trait的类型既可以重写该方法也可以直接调用。
```rust
pub trait Summary {
    fn summarize(&self) -> String {
        String::from("(Read more...)")
    }
}

pub struct NewsArticle {
    pub headline: String,
    pub location: String,
    pub author: String,
    pub content: String,
}

impl Summary for NewsArticle {}

pub struct Tweet {
    pub username: String,
    pub content: String,
    pub reply: bool,
    pub retweet: bool,
}

impl Summary for Tweet {
    fn summarize(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
}

use chapter10::{self, NewsArticle, Summary};

fn main() {
    let article = NewsArticle {
        headline: String::from("Penguins win the Stanley Cup Championship!"),
        location: String::from("Pittsburgh, PA, USA"),
        author: String::from("Iceburgh"),
        content: String::from(
            "The Pittsburgh Penguins once again are the best \
             hockey team in the NHL.",
        ),
    };

    println!("New article available! {}", article.summarize());  // 直接使用默认的方法
}
```
默认的方法可以调用同trait下的其他方法：
```rust
pub trait Summary {
    fn summarize_author(&self) -> String;

    fn summarize(&self) -> String { 
        // 调用了同trait下的其他方法
        format!("(Read more from {}...)", self.summarize_author())
    }
}

pub struct Tweet {
    pub username: String,
    pub content: String,
    pub reply: bool,
    pub retweet: bool,
}

impl Summary for Tweet {
    fn summarize_author(&self) -> String {
        format!("@{}", self.username)
    }
}

use chapter10::{self, Summary, Tweet};

fn main() {
    let tweet = Tweet {
        username: String::from("horse_ebooks"),
        content: String::from(
            "of course, as you probably already know, people",
        ),
        reply: false,
        retweet: false,
    };

    println!("1 new tweet: {}", tweet.summarize());
}

```
## trait作为参数
参数前加`impl`关键字
```rust
pub trait Summary {
    fn summarize(&self) -> String;
}

pub struct NewsArticle {
    pub headline: String,
    pub location: String,
    pub author: String,
    pub content: String,
}

impl Summary for NewsArticle {
    fn summarize(&self) -> String {
        format!("{}, by {} ({})", self.headline, self.author, self.location)
    }
}

pub struct Tweet {
    pub username: String,
    pub content: String,
    pub reply: bool,
    pub retweet: bool,
}

impl Summary for Tweet {
    fn summarize(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
}

pub fn notify(item: &impl Summary) {
    println!("Breaking news! {}", item.summarize());
}
```
### trait Bound语法
```rust
pub fn notify<T: Summary>(item: &T) {
    println!("Breaking news! {}", item.summarize());
}
```
`<T: Summary>`里的`Summary`就是*bound*
### 使用多个trait bound
```rust
pub fn notify(item: &(impl Summary + Display)) {

// or like  this
pub fn notify<T: Summary + Display>(item: &T) {
```
### 多个bound使用`where`字句
多个bound使用`where`字句会使代码更加清晰
```rust
fn some_function<T: Display + Clone, U: Clone + Debug>(t: &T, u: &U) -> i32 {

// 这样写更清晰
fn some_function<T, U>(t: &T, u: &U) -> i32
    where T: Display + Clone,
          U: Clone + Debug
{
```
### 实现Trait的返回值
```rust
pub trait Summary {
    fn summarize(&self) -> String;
}

pub struct NewsArticle {
    pub headline: String,
    pub location: String,
    pub author: String,
    pub content: String,
}

impl Summary for NewsArticle {
    fn summarize(&self) -> String {
        format!("{}, by {} ({})", self.headline, self.author, self.location)
    }
}

pub struct Tweet {
    pub username: String,
    pub content: String,
    pub reply: bool,
    pub retweet: bool,
}

impl Summary for Tweet {
    fn summarize(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
}

fn returns_summarizable() -> impl Summary {
    Tweet {
        username: String::from("horse_ebooks"),
        content: String::from(
            "of course, as you probably already know, people",
        ),
        reply: false,
        retweet: false,
    }
}
```
> 需要注意的是函数中只能返回一种实现trait的类型

### Using Trait Bounds to Conditionally Implement Methods
在任何满足trait bound的类型上的trait的实现称为*blanket implementations*, 这个标准库中很常见
```rust
use std::fmt::Display;

struct Pair<T> {
    x: T,
    y: T,
}

impl<T> Pair<T> {
    fn new(x: T, y: T) -> Self {
        Self { x, y }
    }
}

impl<T: Display + PartialOrd> Pair<T> {
    fn cmp_display(&self) {
        if self.x >= self.y {
            println!("The largest member is x = {}", self.x);
        } else {
            println!("The largest member is y = {}", self.y);
        }
    }
}
```
`Pair<T>`总会有`new()`方法，但是只有当`Pair`的类型为**T**的时候，才会实现`cmp_display`方法
```rust
impl<T: Display> ToString for T {
    // --snip--
}
```
上面的代码表示任何实现了`Display`的类型都可以使用`ToString` trait定义的`to_string()`方法
## Lifetime
Rust中的每一个引用都有个*lifetime*, 也就是引用有效的范围。和变量的类型类似，一般情况下Rust可以推断引用的lifetime，但是当这下引用的lifetime无法相关联时，Rust需要我们通过使用generic lifetime parameters注释这个关系来确保世界的引用在运行时是有效的。
```rust
fn main() {
    let string1 = String::from("abcd");
    let string2 = "xyz";

    let result = longest(string1.as_str(), string2);
    println!("The longest string is {}", result);
}

fn longest(x: &str, y: &str) -> &str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```
上面的这个代码是错误的，因为无法确定传入的引用的具体lifetime，所以我们也无法确定函数返回的引用是否还有效。
### liftime注释语法
下面的代码中，函数的形参和返回值都加了` `a `, 表示的是形参中的所有引用和返回值必须具有相同的lifetime，这个lifetime被命名为` `a `
```rust
fn main() {
    let string1 = String::from("abcd");
    let string2 = "xyz";

    let result = longest(string1.as_str(), string2);
    println!("The longest string is {}", result);
}

fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```
### struct中的lifetime
 ```rust
 struct ImportantExcerpt<'a> {
    part: &'a str,
}

fn main() {
    let novel = String::from("Call me Ishmael. Some years ago...");
    let first_sentence = novel.split('.').next().expect("Could not find a '.'");
    let i = ImportantExcerpt {
        part: first_sentence,
    };
}
 ```
 ### Lifetime Elision
 ### 在方法中使用lifetime
# 面向对象
`struct`和`enum`的成员默认是*private*，使用`pub`可让成员对外可见。
```rust
pub struct AveragedCollection {
    list: Vec<i32>,
    average: f64,
}

impl AveragedCollection {
    pub fn add(&mut self, value: i32) {
        self.list.push(value);
        self.update_average();
    }

    pub fn remove(&mut self) -> Option<i32> {
        let result = self.list.pop();
        match result {
            Some(value) => {
                self.update_average();
                Some(value)
            }
            None => None,
        }
    }

    pub fn average(&self) -> f64 {
        self.average
    }

    fn update_average(&mut self) {
        let total: i32 = self.list.iter().sum();
        self.average = total as f64 / self.list.len() as f64;
    }
}
```
Rust不支持继承

## Trait
```rust
pub trait Draw {
    fn draw(&self);
}

pub struct Screen {
    pub components: Vec<Box<dyn Draw>>,
}
```

