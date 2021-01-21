# 1. 什么是Mybatis
MyBatis 是一款优秀的持久层框架，它支持自定义 SQL、存储过程以及高级映射。MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。
# 2. 使用
1) 创建数据库
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
2) Idea 连接数据库
```sql
show variables like'%time_zone';
set global time_zone='+8:00';
#PS：刚设置完新时区，直接用show variables like'%time_zone';再次查看会发现显示的依然是旧的设置，但实际上是更新了的，可以重启数据库查看或者输入命令（ 该命令用来刷新MySQL的系统权限相关表）
flush privileges;
#后，再次输入
show variables like'%time_zone';
```
3) 在`resources`文件下创建config.xml配置文件
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSH=true&amp;useUnicode=true&amp;characterEncoding=UTF-8&amp;serverTimezone=Asia/Shanghai"/>
                <property name="username" value="root"/>
                <property name="password" value="alex123"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="org/example/dao/UserMapper.xml"/>
    </mappers>
</configuration>
```
4) 创建工具类
```java
public class MybatisUtils {
    private static SqlSessionFactory sqlSessionFactory;
    static {
        try {
            //1. 获取sqlSessionFactory对象
            String resource = "org/mybatis/example/mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e){
            e.printStackTrace();
        }
    }
    // 获得 SqlSession 的实例, SqlSession 提供了在数据库执行 SQL 命令所需的所有方法
    public static SqlSession setSqlSession(){
        return sqlSessionFactory.openSession();
    }

}
```
5) 创建实体类 alter + insert
6) Dao接口
```java
public interface UserDao {
    List<User> getUserList();
}
```
7) 实现Dao接口
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="org.example.dao.UserDao">
    <!-- id 对应UserDao方法名-->
    <select id="getUserList" resultType="org.example.pojo.User">
        select * from mybatis.user
    </select>
</mapper>
```
7) 测试 Junit
```java
public class UserDaoTest {
    @Test
    public void test(){
        // 第一步：获取sqlSession对象
        SqlSession sqlSession = MybatisUtils.getSqlSession();

        // 方式1：getMapper
        UserDao mapper = sqlSession.getMapper(UserDao.class);
        List<User> userList = mapper.getUserList();

        for (User user : userList) {
            System.out.println(user);
        }

        // 关闭sqlSession
        sqlSession.close();
    }
}
```
# 3. GRUD
namespace里的包名要和Mapper接口里的一致
## Select
- id 对应的就是namespace的方法名
- resultType: sql语句的返回值的类型
- parameterType: 参数类型
```xml
    <select id="getUserById" parameterType="int" resultType="org.example.pojo.User">
        select * from mybatis.user where id = #{id}
    </select>
```
## Insert
```xml
    <insert id="addUser" parameterType="org.example.pojo.User" >
        insert into mybatis.user (id, name, pwd) values (#{id}, #{name}, #{pwd})
    </insert>
```
## Update
```xml
    <update id="updateUser" parameterType="org.example.pojo.User">
        update mybatis.user set name = #{name}, pwd = #{pwd} where id = #{id}
    </update>
```
## delete
```xml
    <delete id="deleteUser" parameterType="int">
        delete from mybatis.user where id = #{id}
    </delete>
```
> 增删改需要提交事务 `sqlSession.commit();`
## 使用Map
若实体类的字段太多，可使用Map进行操作。或者用**注解**！
```java
    // 使用Map
    int addUser2(Map<String, Object> map);
```
```xml
    <insert id="addUser2" parameterType="map">
        insert into mybatis.user (id, pwd) values (#{userId}, #{userPassword})
    </insert>
```
```java
    @Test
    public void testAddUser2(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        Map<String, Object> map = new HashMap<>();

        // key要和映射文件里配置的一致
        map.put("userId", 5);
//        map.put("userName","Elon");
        map.put("userPassword","567");
        mapper.addUser2(map);
        sqlSession.commit();
        sqlSession.close();
    }
```
### 模糊查询
- 方法一：在java代码中使用通配符`%%`
```java
List<User> users = sqlSession.getMapper(UserMapper.class).getUserLike("%a%");
```
- 方法二：在配置文件里设置(推荐)
```xml
    <select id="getUserLike" resultType="org.example.pojo.User">
        select * from user where name like "%"#{value}"%"
    </select>
```
# 配置优化
## 2. 环境配置（environments） 
Mybatis的默认事务管理器是JDBC，连接池POOLED
## 3. 属性（properties）
这些属性可以在外部进行配置，并可以进行动态替换。既可以在典型的 Java 属性文件中配置这些属性，也可以在 properties 元素的子元素中设置。(db.properties)
- 编写配置文件db.properties
```properties
driver=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatis?useSSH=true;useUnicode=true;characterEncoding=UTF-8;serverTimezone=Asia/Shanghai
username=root
password=alex123
```
- 在核心配置文件中引入
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <properties resource="db.properties"/>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="org/example/dao/UserMapper.xml"/>
    </mappers>
</configuration>
```
> XML文件中每个元素的顺序要严格遵守，不能错
##  4. 类型别名（typeAliases）
类型别名可为 Java 类型设置一个缩写名字。 它仅用于 XML 配置，意在降低冗余的全限定类名书写
- 方法1：
```xml
    <typeAliases>
        <typeAlias type="org.example.pojo.User" alias="User"/>
    </typeAliases>
```
- 方法二：指定一个包名，MyBatis 会在包名下面搜索需要的 Java Bean
```xml
<typeAliases>
  <package name="domain.blog"/>
</typeAliases>
```
每一个在包 domain.blog 中的 Java Bean，在没有注解的情况下，会使用 Bean 的首字母小写的非限定类名来作为它的别名。 比如 domain.blog.Author 的别名为 author；若有注解，则别名为其注解值。
```java
@Alias("author")
public class Author {
    ...
}
```
## 5. 设置（settings）
## 6. 插件（plugins）
- MybatisPlus
## 7. 映射器（mappers
- 方式1：
```xml
<!-- 使用相对于类路径的资源引用 -->
<mappers>
  <mapper resource="org/mybatis/builder/AuthorMapper.xml"/>
  <mapper resource="org/mybatis/builder/BlogMapper.xml"/>
</mappers>
```
- 方式2：
```xml
<!-- 使用映射器接口实现类的完全限定类名 -->
<mappers>
  <mapper class="org.mybatis.builder.AuthorMapper"/>
  <mapper class="org.mybatis.builder.BlogMapper"/>
</mappers>
```
注意点：
- 接口和它的Mapper配置文件必须同名
- 接口与它的Mapper配置文件必须在同一包下
