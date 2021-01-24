- [手册](https://docs.spring.io/spring-framework/docs/5.2.0.RELEASE/spring-framework-reference/index.html)
- [发行](https://repo.spring.io/release/org/springframework/spring/)

依赖:
```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.3.3</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.3.3</version>
</dependency>
```
## 优点
- 控制反转(IOC) 面向切片(AOP)
- 支持事务的处理
## 2. Spring组成
由七大模块组成
# IOC理论推导
- UserDao接口
- UserDaoImpl实现类
- UserService 业务接口
- UserSericeImpl业务实现类

在之前的业务中，用户的需求可能会影响我们原来的代码，我们需要根据用户的需求去修改原代码！如果程序代码量十分大，修改一次的成本代价十分昂贵！

我们使用一个Set接口实现
```java
// 不用在这里new对象了，可交给用户自己传入
private UserDao userDao;
public void setUserDao(UserDao userDao) {
    this.userDao = userDao;
}
```
- 之前程序是主动创建对象，控制权在程序员手上
- 使用了set注入后，程序不再具有主动性，而变成了被动的接受对象

这种思想从本质解决了问题，程序员不用再去管理对象的创建了，系统的耦合性大大降低，可以更加专注再业务层的实现上，这是IOC的原型！
## IOC本质
控制反转(IOC)是一种设计思想，依赖注入(DI)是实现Ioc的一种方法。没有Ioc的程序中，我们使用面向对象编程，对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方。控制反转就是：获得依赖对象的方式反转了。

采用XMl方式配置Bean的时候，Bean的定义信息和实现分离的，而采用注解的方式可以把两者合为一体，Bean的定义信息直接以注解的形式定义在实现类中，从而达到了零配置的目的。

控制反转是一种通过描述(XML或注解)并通过第三方去生产或获取特定对象的方式。在Spring中实现控制反转的是Ioc容器，其实现方法是依赖注入(Dependency Injection)。
