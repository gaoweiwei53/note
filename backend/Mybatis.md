# 1. 什么是Mybatis
MyBatis 是一款优秀的持久层框架，它支持自定义 SQL、存储过程以及高级映射。MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。
# 2. 使用
1.1 创建数据库
```sql
CREATE DATABASE mybatis;
USE mybatis;
CREATE TABLE IF NOT EXISTS user(
	id INT NOT NULL,
    name VARCHAR(30) DEFAULT NULL,
    pwd VARCHAR(30) DEFAULT NULL,
    PRIMARY KEY(id)
)ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;  # 设置数据库引擎和字符编码
SHOW TABLES;

INSERT INTO user(id, name, pwd) VALUES
(1, "alex", "123"), 
(2, "bob", "234"),
(3, "cindy","345");

SELECT * FROM user;
```
1.2 Idea 连接数据库
```sql
show variables like'%time_zone';
set global time_zone='+8:00';
#PS：刚设置完新时区，直接用show variables like'%time_zone';再次查看会发现显示的依然是旧的设置，但实际上是更新了的，可以重启数据库查看或者输入命令（ 该命令用来刷新MySQL的系统权限相关表）
flush privileges;
#后，再次输入
show variables like'%time_zone';
```
