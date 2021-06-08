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

