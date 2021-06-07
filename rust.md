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

