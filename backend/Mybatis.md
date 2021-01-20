# 1. 什么是Mybatis
MyBatis 是一款优秀的持久层框架，它支持自定义 SQL、存储过程以及高级映射。MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。
# 使用
1. 创建数据库
```sql
CREATE DATABASE mybatis;
USE mybatis;
CREATE TABLE IF NOT EXISTS user(
	id INT NOT NULL,
    name VARCHAR(30) DEFAULT NULL,
    pwd VARCHAR(30) DEFAULT NULL,
    PRIMARY KEY(id)
)ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
SHOW TABLES;

INSERT INTO user(id, name, pwd) VALUES
(1, "alex", "123"), 
(2, "bob", "234"),
(3, "cindy","345");

SELECT * FROM user;
```
