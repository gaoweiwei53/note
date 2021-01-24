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
## Spring组成
由七大模块组成
# 2. IOC理论推导
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
## IOC创建对象的方式
1) 使用无参构造创建对象，默认
2) 假设我们要使用有参构造创建
- 下标赋值
```xml
<bean id="user" class="org.example.pojo.User">
    <constructor-arg index="0" value="Data"/>
</bean>
```
- 通过类型传值 不建议使用，若由几个相同类型的参数，则无法使用
```xml
<bean id="user" class="org.example.pojo.User">
    <constructor-arg type="java.lang.String" value="Houmeji"/>
</bean>
```
- 通过参数名
```xml
<bean id="user" class="org.example.pojo.User">
    <constructor-arg name="name" value="Hasagei"/>
</bean>
```

总结：在配置文件加载的时候，容器中管理的对象就已经初始化了
# 3. Spring配置
## 3.1 别名
`<alias name="user" alias="user2"/>`
## 3.2 Bean的配置
- `name`也可以取别名，且功能更强大
```
<bean id="user" class="org.example.pojo.User" name="user3 user4">
    <constructor-arg name="name" value="Hasagei"/>
</bean>
```
## 3.3 Import
一般用于团队开发，可将多个配置文件导入合并为一个
# 4. 依赖注入
## 4.1 构造器注入
## 4.2 Set注入(重点)
- 依赖：bean对象的创建依赖于容器
- 注入：bean对象中的所有属性，由容器注入  
```xml
 <bean id="address" class="org.example.pojo.Address">
      <property name="address" value="Shanghai"/>
  </bean>
  <bean id="student" class="org.example.pojo.Student">
      <!-- 1 normal value       -->
      <property name="name" value="Spring"/>

      <!-- 2 bean ref     -->
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
## 4.3 扩展方式注入
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
