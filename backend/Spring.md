# 1. 前言
在.properties配置文件中写入数据库配置数据，username=root。username是环境变量，spring在对配置文件解析后会直接读取环境变量为我自己电脑用户。在配置文件中修改username=jdbc.username即可
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
## Java创建对象的方式
1) 构造方法
2) 反射
3) 序列化
4) 克隆
5) IOC
6) 动态代理
## 优点
- 控制反转(IOC) 面向切片(AOP)
- 支持事务的处理
## Spring组成
由七大模块组成
## junit
- 单元：指定的是方法，一个类有很多方法，一个方法称为单元
### 创建测试方法
1) public方法
2) 没有返回值
3) 方法名自定义
4) 方法没有参数
5) 方法上面加入@test, 这样的方法可以单独执行，不需要main方法
# 2. IOC
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
## 2.1 IOC本质
控制反转(IOC)是一种设计思想，依赖注入(DI)是实现Ioc的一种方法。没有Ioc的程序中，我们使用面向对象编程，对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方。控制反转就是：获得依赖对象的方式反转了。

采用XMl方式配置Bean的时候，Bean的定义信息和实现分离的，而采用注解的方式可以把两者合为一体，Bean的定义信息直接以注解的形式定义在实现类中，从而达到了零配置的目的。

控制反转是一种通过描述(XML或注解)并通过第三方去生产或获取特定对象的方式。在Spring中实现控制反转的是Ioc容器，其实现方法是依赖注入(Dependency Injection)。
## 2.2 IOC创建对象的方式
- 告诉spring创建对象：声明`bean`，就是告诉spring要创建某个类的对象。
- `id`: 对象的自定义名称，spring通过这个名称找到对象。
- `class`: 类的全限定名称(不能是接口，因为spring是反射创建对象，必须使用类)

spring是把创建好的对象放入到map中，spring框架有一个map创建对象。其中`id`就是map的key，value就是对象
## 2.3 获取对象
- `ApplicationContext`就表示spring容器
- `ClassPathXmlApplicationContext`:表示从类路径中加载spring的配置文件
```java
ApplicationContext ac = new ClassPathXmlApplicationContext(config);
ac.gtBean("id")
int nums = ac.getBeanDefinitionCount(); //获取对象的数量
String names[] = ac.getDefinitonNames();// 获取每个对象的名称
```
## 2.4 创建对象的时机
执行`ApplicationContext ac = new ClassPathXmlApplicationContext(config);`,`ClassPathXmlApplicationContext`读取配置文件时，创建了对象。
## 2.5 Import
一般用于团队开发，可将多个配置文件导入合并为一个
```xml
<import resource="classpath:**/*.xml"/>
```
可使用通配符一次加载多个文件
# 3. 依赖注入
## 3.1 给对象的属性赋值
有两种方法
1) 使用XML配置文件(理解即可)
2) 使用注解(掌握，常使用)
### 基于XML的DI
Bean 实例在调用无参构造器创建了空值对象后，就要对 Bean对象的属性进行初始化。初始化是由容器自动完成的，称为**注入**。根据注入方式的不同，常用的有两类：set注入、 构造注入
1) 构造器注入
使用有参构造方法注入
- 下标赋值
```xml
<bean id="user" class="org.example.pojo.User">
    <constructor-arg index="0" value="Data"/>
</bean>
```
通过类型传值 不建议使用，若由几个相同类型的参数，则无法使用
```xml
<bean id="user" class="org.example.pojo.User">
    <constructor-arg type="java.lang.String" value="Houmeji"/>
</bean>
```
通过参数名
```xml
<bean id="user" class="org.example.pojo.User">
    <constructor-arg name="name" value="Hasagei"/>
</bean>
```
2) Set注入(重点，80%都使用set注入)
本质调用set方法赋值  
- 依赖：bean对象的创建依赖于容器
- 注入：bean对象中的所有属性，由容器注入  
```xml
 <bean id="address" class="org.example.pojo.Address">
      <property name="address" value="Shanghai"/>
  </bean>
  <bean id="student" class="org.example.pojo.Student">
      <!-- 1 normal value       -->
      <property name="name" value="Spring"/>

      <!-- 2 bean ref address也需要用bean声明     -->
      <property name="address" ref="address"/>

      <!-- 3 Array        -->
      <property name="books">
          <array>
              <value>Little Princess</value>
              <value>Bird</value>
              <value>Dog</value>
          </array>
      </property>

      <property name="hobbys">
          <list>
              <value>Listen to music</value>
              <value>Coding</value>
              <value>Play football</value>
          </list>
      </property>

      <property name="card">
          <map>
              <entry key="ID card" value="123456"/>
              <entry key="Car Card" value="654321"/>
          </map>
      </property>

      <property name="games">
          <set>
              <value>LOL</value>
              <value>COC</value>
          </set>
      </property>
      <property name="wife">
          <null/>
      </property>

      <property name="info">
          <props>
              <prop key="StudentID">1971669</prop>
          </props>
      </property>
  </bean>
```
## 3.2 自动注入(只能针对引用类型有效，简单类型必须手动赋值)
### ByName自动注入
- 会自动在容器上下文中查找，和自己对象set方法后面的值对应的bean id  
- Java中的属性名必须和配置文件中<bean> 的 id名称一样且类型一致！
```xml
<bean id="cat" class="org.example.pojo.Cat"/>
<bean id="dog" class="org.example.pojo.Dog"/>
<bean id="people" class="org.example.pojo.People" autowire="byName"/>
```
    
### ByType自动注入
java类中引用类型和Spring配置文件中<bean>的class属性是同源关系的，这样的bean能够赋值给引用类型。
- 类型一样
- 类型是父子关系
- 类型是接口和继承的关系
```xml
<bean id="cat" class="org.example.pojo.Cat"/>
<bean id="dog" class="org.example.pojo.Dog"/>
<bean id="people" class="org.example.pojo.People" autowire="byType"/>
```
小结：
- byname需要保证所有bean的id唯一，并且这个bean需要和自动注入的属性的set方法的值一致
- bytype需要保证所有bean的class唯一，并且这个bean需要和自动注入的属性的类型一致！
    
## 3.3 扩展方式注入
我们可以使用p命名空间和c命名空间进行注入
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="user" class="org.example.pojo.User" p:name="Xiaoming" p:age="18"/>
    <!-- use constructor to DI    -->
    <bean id="user2" class="org.example.pojo.User" c:name="xiaohua" c:age="19"/>
</beans>
```
#  5. Bean的作用域
- singleton Spring的默认模式
- prototype 每次从容器中get的时候都会产生新的对象
- request
# 6. Bean的自动装配
- Spring会在上下文自动寻找并自动给bean装备属性

在Spring中有三种装配的方式
1) 在xml中显示的配置
2) 在Java中显示配置
3) 自动装配bean【重要】

## 6.3 使用注解实现自动装配
- 导入约束：context约束
- 配置注解的支持：`<context:annotation-config/>`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>
</beans>
```

# 7. 使用注解开发
- 在Spring4之后，使用注解开发必须要保证Aop的包导入了
- 使用注解需要导入context约束，增加注解的支持

使用注解的步骤：
1) 加入依赖spring-context，在加入spring-context的同时，间接加入spring-aop的依赖
2) 在类中加入spring的注解
3) 在配置文件中加入组件扫描器的标签，说明注解在项目中的位置

> @Component @Repository @Service @Controller @Value @AutoWired @Resource
## 7.1 定义Bean的注解@@Component
1) 在类中加入spring的注解
```java
/**
 * @Component: 创建对象的，等同于<bean>的功能
 *  属性：value 就是对象的名称，相当于<bean> 的id
 *  位置：类的上面
 * 若不知道名称，默认名称为类名的小写名字
 */
@Component(value="myUser")
public class User {
    @Value("Spring")
    public String name;
}
```
2) 在配置文件中加入组件扫描器的标签，说明注解在项目中的位置
```xml
<context:component-scan base-package="类的全限定名"
```
### 指定多个包的三种方式
```xml
<!--方法1-->
<context:component-scan base-package="org.example.dao1"/>
<context:component-scan base-package="org.example.dao2"/>

<!--方法2, 使用分号或逗号-->
<context:component-scan base-package="org.example.dao1;org.example.dao2;"/>

<!--方法3：指定父包-->
<context:component-scan base-package="org.example"/>
```
### @Component衍生的注解
@Component有几个衍生注解，在web开发中，会按照mvc三层架构分层，并且它们还有额外的功能
- dao `@Repository`
- service `@Service`
- controller  `@Controller`
## 7.2 简单类型的属性注入@Value
- 属性：value 表示简单类型的属性值
- 位置：
    - 在属性定义的上面，无需set方法，推荐使用
    - 在set方法上面
```java
@Component(value="myUser")
public class User {
    @Value("Spring")
    public String name;
}
```
## 7.3 引用类型的属性注入@AutoWired或@Resource
### @Autowired
@AutoWired实现引用类型的赋值，使用的是自动注入原理，支持byName，byType.默认使用的是byType自动注入
- 直接在属性上使用
- 使用Autowired可以不用编写Set方法
- `required`为false表示这个对象可以为null，否则不允许为空
- 如果@Autowire自动装配的环境比较复杂，自动装配无法通过一个注解@Autowired完成的时候，可以使用@Qualifier("")去配合@Autowired使用，指定一个唯一的bean对象注入。
```java
public class People {
    @Autowired
    private Cat cat;
    @Autowired
    @Qualifier("dog22")
    private Dog dog;
    private String name;
}
```
## 7.4 自动装配置
- @Autowired: 自动装配通过类型，名字
- @Nullable:
- @Resource:
## 7.5 作用域
`@Scope("singleton")`
## 7.6 小结
- XML与注解：
    - XML更加万能，适用于任何场合！维护简单方便
    - 注解不是自己类是用不了，维护相对复杂
- XML与注解最佳实践
    - XML用来管理bean
    - 注解只负责完成属性注入
## 7.7 使用Java方式配置Spring
在Spring4之后成为了核心功能！

配置类
```java
@Configuration
@ComponentScan("org.example.pojo")
@Import(MyConfig2.class)  //导入其他配置
public class MyConfig {
    // 方法的名字相当于bean配置文件里的 id属性
    // 返回值相当于bean配置文件里的 class属性
    @Bean
    public User getUser(){
        return new User(); //就是要注入到bean中的对象
    }
}
```
测试类
```java
public class MyTest {
    @Test
    public void test1(){
        ApplicationContext context = new AnnotationConfigApplicationContext(MyConfig.class);
        User user = (User) context.getBean("getUser");
        System.out.println(user.getName());
    }
}
```
# 8. 代理模式
这就是AOP的底层

代理模式的分类：
- 静态代理
- 动态代理
## 8.1 静态代理
角色分析：
- 抽象角色：一般使用接口或抽象类来解决
- 真实角色：被代理的角色
- 代理角色：代理真实的角色，一般会做一些附属的操作
- 客户：访问代理对象的人

代理模式的好处：
- 可以使真实角色的操作更加纯粹，不用关注一些公共的业务
- 公共业务交给代理角色，实现了业务的分工
- 公共业务发生扩展的时候，方便集中管理

缺点：
- 一个真实角色就会产生一个代理角色；代码量会翻倍，开发效率变低
## 8.2 动态代理
- 动态代理和静态代理角色一样
- 动态代理的代理类是动态生成的，不是直接写好的
- 动态代理分为两大类：基于接口的动态代理，基于类的动态代理
    - 基于接口  JDK动态代理
    - 基于类  cglib
    - java字节码实现  javasist
# 9. AOP
## 9.1 什么是AOP
AOP(Aspect Oriented Programming)面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使业务逻辑各部分之间的耦合读降低，提高程序的可重用性，同时提高开发的效率。 

方式1：使用Spring的API接口
```xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.6</version>
</dependency>
```
方式2：使用自定义实现AOP
# 10. Spring-Mybatis
## 10.1 回忆Mybatis
- 编写实体类
- 编写核心配置文件
- 编写接口
- 编写Mapper.xml
- 测试

[Mybatis-Spring](http://mybatis.org/spring/zh/index.html)

# 11. 声明式事务
## 事务ACID原则
- 原子性
- 一致性
- 隔离性
- 持久性
## Spring中的事务管理
- 声明式事务：AOP
- 编程式事务：需要在代码中进行事务的管理
处理事务需要做什么
1) 事务管理器是一个接口和他的众多实现类。接口PlatformTransactionManager，定义了重要的方法commit,rollback.
    - Myabtis访问数据库使用DataSourceTransanctionManager
你需要告诉spring用的是哪种数据库的访问技术，声明数据库访问技术对应的事务管理器实现类，在spring的配置文件中使用<bean>声明就可以了
```xml
    <bean id="xxx" class="...DataSourceTransanctionManager"
```
2) 业务方法需要什么样的事务，需要说明事务的类型
    1) 事务的隔离级别：有四个值
    2) 事务的超时时间: 表示方法最长的执行时间
    3) 传播行为：控制业务方法是不是有事务的，是什么样的事务。有7个传播行为，表示你的业务调用时，事务在方法之间是如何使用的，掌握三个
          - REQUIRED: 指定的方法必须在事务内执行。若当前存在事务，就加入到事务当中，若没有事务则创建新的事务，默认选择
          - REQUIRES_NEW: 总是新建一个事务，若当前存在事务，就将当前事务挂起，直到新事务执行完毕。
          - SUPPORTS：指定的方法支持当前事务，但若当前没有事务，也可以以非事务方式执行
3) 提交事务，回滚事务的时机
    - 当业务方法执行成功，没有异常抛出，方法执行完毕时，spring提交事务
    - 当业务方法抛出运行时异常，spring执行回滚，调用事务管理器的rollback.运行时异常的定义：RuntimeExcception和它的子类都是运行时异常。
    - 当业务方法抛出非运行时异常，主要是受查异常时，提交事务。受查异常：在代码中必须处理的异常，例如IOExccception，SQLException
          
### Spring中提供的事务处理方案
1) 适合中小项目使用，注解方案
    Spring框架自己用aop实现给业务方法增加事务的功能，使用@Transactional注解增加事务。放在publi方法的上面，表示当前方法具有事务。可以给注解的属性赋值，表示具体的隔离级别，传播行为，异常信息等。
    
使用@Transactional的步骤：
 1) 需要声明事务管理器对象` <bean id="xxx" class="...DataSourceTransanctionManager"`
 2) 开启事务注解驱动，告诉spring框架，要使用注解的方式管理事务
 3) 在方法上加入@Transactional
2) 适合大型项目，有很多的类、方法，需要大量的配置事务，使用aspectj框架，在spring配置文件中声明，方法需要的事务，这种方式业务方法和事务配置完全分离。
