# 1. Tomcat
## 1.1 Http
### http响应
- 响应体
```json
Accept-Ranges:
Cache-Control: 
Content-Length:
Content-Type: 
Connection:
Content-Encoding:
Date:
Etag: 
Server:
```
- 响应状态码
### GET请求
- 请求行
  - 请求的方式
    - `GET`: 能够携带的参数比较少，大小有限制，会在浏览器的URL地址栏显示数据内容，不安全，但高效
    - `POST`: 携带的参数没有限制，大小没有限制，不会在浏览器的URL地址栏显示数据内容，安全，但不高效
  - 请求的资源路径
  - 请求的协议的版本号
- 请求头
  - Accept: 客户端可以接收的数据类型
  - Accept-Language: 客户端可以接收的语言
  - User-Agent: 浏览器的信息
  - Accept_Encoding: 客户端可以接收的数据编码格式
  - Host: 请求的服务器和端口号
  - Connection: 告诉服务器请求如何处理
    - Keep-Alive 保持一小时间段的连接
    - Closed 马上关闭
### Post请求
- 请求行
- 请求头
  - Accept: 客户端可以接收的数据类型
  - Accept-Language: 客户端可以接收的语言
  - User-Agent: 浏览器的信息
  - Content-type：表示发送的数据类型
    - application/x-www-form-urlencodered 
    表示提交的数据格式是：name=value&name=value, 然后对其进行url编码
    url编码是把非英文的内容转化为：%xx%xx
    - mutilpart/form-data
    表示以多段的形式提交给服务器
  - content-Length: 发送的数据长度
  - Cache-Control：表示如何控制缓存，no-cache不缓存
  
- 空行
- 请求体：发送给服务器的数据

## 1.2 常见的响应码说明
- 200 请求成功
- 302 重定向 
- 404 数据不存在
- 500 服务器内部错误

## 1.3 base标签
base标签可以设置页面相对路径的 参照路径
    
  
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
1) 编写一个类继承HttpServlet类
2) 重写doGet或doPost方法
3) 在web.xml中配置Servlet程序访问地址

> 可使用idea 直接创建Servlet程序
## 2.5 ServletConfig类
Servlet程序的配置信息
### ServletConfig类的三大作用
1) 可以获取Servlet程序的别名 servlet-name的值
2) 获取初始化参数 init-param
3) 获取ServletContext对象

Servlet程序和Servlet对象都是由Tomcat负责创建，我们负责使用。

Servlet程序默认是第一次访问的时候创建，ServletConfig是每个Servlet程序创建时就创建一个对应的ServletConfig对象。

> 重写init方法时，必须要加`super.init(config);`
## 2.6 ServletContext类
- ServletContext是一个接口，表示Servlet上下文对象
- 一个web应用对应的是一个ServletContext对象
- 由于一个 web 应用程序的所有 Servlet 都共享的是同一个 ServletContext 对象，所以 ServletContext 对象也被称为 application 对象（web 应用程序对象)
- 共享数据
在一个Servlet中放一个数据，可以在另一个Servlet中获取
### ServletContext类的四个作用
1) 获取虚拟路径所映射的本地路径 
  - 虚拟路径：浏览器访问 web 应用中资源时所使用的路径 
  - 本地路径：资源在文件系统中的实际保存路径
2) application 域范围的属性
3) 获取 web.xml这种配置的上下文参数 context-param
## 2.7 HttpServletRequest 接口
该接口是 ServletRequest 接口的子接口，封装了 HTTP 请求的相关信息，由 Servlet 容器创建 其实现类对象并传入 service(ServletRequest req, ServletResponse res)方法中。我们请求的详细 信息都可以通过 HttpServletRequest 接口的实现类对象获取。这个实现类对象一般都是容器 创建的，我们不需要管理。
### HttpServletRequest 主要功能
- 获取请求参数
  - `getParameter()` 获取请求的参数，单值
  - `getParameterValues() ` 获取请求的参数，多值
- 转发页面
  - `getRequestDispatcher("servlet2")`
- 获取请求头相关信息
## 2.8 HttpServletResponse 接口
HttpServletResponse 是 ServletResponse 接口的子接口，封装了 HTTP 响应的相关信息，由 Servlet 容器创建其实现类对象并传入 service(ServletRequest req, ServletResponse res)方法中。设置返回客户端的消息可以通过HttpServletResponse设置。
### 功能
#### 1. 向浏览器发送数据
- 字节流 `getOutPutStream()` 常用于下载
- 字符流 `getWrite()` 常用于传递字符串  
两者只能使用其中一个
- 使用 PrintWriter 对象向浏览器输出数据
#### 2. 向浏览器发送响应头的方法

#### 3. 响应状态码
- 实现请求重定向
  - `response.sendRedirect("http://localhost:8080")`
#### 常见功能
1) 向浏览器输出消息
2) 下载文件
  - 获取下载文件的路径
  - 获取下载的文件名
  - 设置让浏览器支持下载的东西
  - 获取下载文件的输入流
  - 创建缓冲区
  - 创建OutPutStream对象
  - 将FileOutputStream流写入Buffer缓冲区
  - 使用OutPutStream将缓冲区里的数据写入到客户端
# 2. JSP(Java Server Page)
Jsp主要作用是代替Servlet程序回传html页面的数据，因为Servlet程序回传html页面的数据是一件非常繁琐的事情，开发成本和维护成本都很高。

> 其本质是Servlet程序，第一次使用的时候会被编译成class文件，该类继承了HttpServlet类
## 2.1 Jsp指令
JSP指令用来设置整个JSP页面相关的属性，如网页的编码方式和脚本语言。

语法格式如下：
```jsp
<%@ directive attribute="value" %>
```
JSP中三种指令标签：
- `<%@ page ... %>` 定义网页依赖属性，比如脚本语言、error页面、缓存需求等等
- `<%@ include ... %>` 包含其他文件
- `<%@ taglib ... %>`  引入标签库的定义
## 2.2 Jsp语法
1) 脚本程序
```jsp
<% 代码片段 %>
```
2) JSP声明的语法格式：
```jsp
<%! int i = 0; %> 
<%! int a, b, c; %> 
<%! Circle a = new Circle(2.0); %> 
```
> 现在基本上不使用了
3) JSP表达式的语法格式：
```jsp
<%= 表达式 %>
```
- 表达式语句被翻译成`out.print()`方法
- `_jspService()`方法中的变量，在表达式中使用
- 语句不能以分号结尾
