# 1. Tomcat
## 1.1 Http
- 请求方式：`GET`, `HEAD`, `POST`
  - `GET`: 能够携带的参数比较少，大小有限制，会在浏览器的URL地址栏显示数据内容，不安全，但高效
  - `POST`: 携带的参数没有限制，大小没有限制，不会在浏览器的URL地址栏显示数据内容，安全，但不高效
  
# 2. Servet技术
## 2.1 什么是Sevlet
- Servlet是JavaEE规范之一。本质是接口。
- Servlet是JavaWeb三大组件之一。三大组件分别是：Servlet程序、Filter过滤器，Listener监听器。
- Servlet是运行在服务器上的一个java小程序，它可以接收客户端发过来的请求，并响应数据给客户端。

## 2.2 手动实现Servlet程序
1) 编写一个类实现Servlet接口
2) 实现service方法，处理请求，并响应数据
3) 到web.xml中配置servlet程序的访问地址
```xml
    <!-- 配置Tomcat servlet程序-->
    <servlet>
        <!--程序的别名，一般就为类名-->
        <servlet-name>MyHelloServlet</servlet-name>
        <!--程序的别名，一般就为类名-->
        <servlet-class>com.example.servlet_01.MyHelloServlet</servlet-class>
    </servlet>
    <!--给Servlet程序配置访问地址-->
    <servlet-mapping>
        <!--告诉服务器当前配置的地址给哪个Servlet程序使用-->
        <servlet-name>MyHelloServlet</servlet-name>
        <!-- / 斜杠在服务器解析的时候，表示地址为工程路径
        -->
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>
```
## 2.3 Servlet的生命周期
1) 执行构造方法
2) 执行init初始化方法
第一、二步第一次访问的时候会调用
3) 执行service方法
第三步每次访问都会调用
4) 执行destroy销毁方法
第四步程序停止时调用
## 2.4 通过继承HttpServlet实现Servlet
> 实际开发的时候很少直接继承Servlet接口
