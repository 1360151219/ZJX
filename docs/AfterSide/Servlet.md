# Servlet

?> **Servlet** 是运行在 Web 服务器或应用服务器上的程序，
它是作为来自 Web 浏览器或其他 HTTP 客户端的请求和 HTTP 服务器上的数据库或应用程序之间的中间层。

## Servlet技术

---

### 什么是Servlet

1、Servlet 是JavaEE 规范之一。规范就是接口

2、Servlet 就JavaWeb 三大组件之一。三大组件分别是：Servlet 程序、Filter 过滤器、Listener 监听器。

3、Servlet 是运行在服务器上的一个java 小程序，它可以接收客户端发送过来的请求，并响应数据给客户端。

---

### 简单实现一个Servlet程序
(实现前需配置好Tomcat，这里不做赘述)

1、编写一个类去实现Servlet 接口 

   实现service 方法，处理请求，并响应数据
```java
public class HelloServlet implements Servlet {
/**
* service 方法是专门用来处理请求和响应的
* @param servletRequest
* @param servletResponse
* @throws ServletException
* @throws IOException
  */
  @Override
  public void service(ServletRequest servletRequest, ServletResponse servletResponse) 
          throws ServletException, IOException { 
      System.out.println("Hello Servlet 被访问了");
  }
}
```



2、到web.xml 中去配置servlet 程序的访问地址
```xml

<!-- servlet 标签给Tomcat 配置Servlet 程序-->
<servlet>
    <!--servlet-name 标签Servlet 程序起一个别名（一般是类名） -->
    <servlet-name>HelloServlet</servlet-name>
    <!--servlet-class 是Servlet 程序的全类名-->
    <servlet-class>com.zhiguo.servlet.HelloServlet</servlet-class>
</servlet>

<!--servlet-mapping 标签给servlet 程序配置访问地址-->
<servlet-mapping>
    <!--servlet-name 标签的作用是告诉服务器，我当前配置的地址给哪个Servlet 程序使用-->
    <servlet-name>HelloServlet</servlet-name>
    <!--url-pattern 标签配置访问地址<br/>
        / 斜杠在服务器解析的时候，表示地址为：http://ip:port/工程路径<br/>
        /hello 表示地址为：http://ip:port/工程路径/hello <br/>
        -->
    <url-pattern>/hello</url-pattern>
</servlet-mapping>
```

---


### url 地址到 Servlet 程序的访问

![](AfterSide_imgs/2.jpg)

---


### Servlet 的生命周期

- 执行Servlet 构造器方法


- 执行init 初始化方法
  (执行构造器和初始化方法是在第一次访问的时候创建 Servlet 程序会调用。)
```java
@Override
    public void init(ServletConfig servletConfig) throws ServletException {

    }
```



- 执行Service 方法
  (执行Servlet方法 每次访问都会调用。)
```java
 @Override
public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {

        }
```


- 执行destroy 销毁方法
  (销毁方法，在web工程停止的时候调用。)
```java
 @Override
public void destroy() {

        }
```

---


### 通过继承 HttpServlet 实现Servlet 程序

一般在实际项目开发中，都是使用继承HttpServlet 类的方式去实现Servlet 程序。

1、编写一个类去继承HttpServlet 类

2、根据业务需要重写doGet 或doPost 方法
```java
public class HelloServlet2 extends HttpServlet {
/**
* doGet（）在get 请求的时候调用
* @param req
* @param resp
* @throws ServletException
* @throws IOException
  */
  @Override
  protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
  IOException { 
      System.out.println("HelloServlet2 的doGet 方法");
  }
  /**
* doPost（）在post 请求的时候调用
* @param req
* @param resp
* @throws ServletException
* @throws IOException
  */
  @Override
  protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
  IOException { 
      System.out.println("HelloServlet2 的doPost 方法");
  }
}
 ```

3、到web.xml 中的配置Servlet 程序的访问地址

```xml
<servlet>
    <servlet-name>HelloServlet2</servlet-name>
    <servlet-class>com.zhiguo.servlet.HelloServlet2</servlet-class>
</servlet>

<servlet-mapping>
    <servlet-name>HelloServlet2</servlet-name>
    <url-pattern>/hello2</url-pattern>
</servlet-mapping>
```

---


### Servlet 类的继承体系

![](AfterSide_imgs/3.jpg)

## ServletConfig 类

---


### 什么是ServletConfig 类

Servlet 程序和ServletConfig 对象都是由Tomcat 负责创建，我们负责使用。

Servlet 程序默认是第一次访问的时候创建，ServletConfig 是每个Servlet 程序创建时，就创建一个对应的ServletConfig 对
象。

---


### ServletConfig 类的三大作用
- 可以获取Servlet 程序的别名servlet-name 的值

- 获取初始化参数init-param

- 获取ServletContext 对象

web.xml 中的配置：
```xml
<!-- servlet 标签给Tomcat 配置Servlet 程序-->
<servlet>
    <!--servlet-name 标签Servlet 程序起一个别名（一般是类名） -->
    <servlet-name>HelloServlet</servlet-name>
    <!--servlet-class 是Servlet 程序的全类名-->
    <servlet-class>com.zhiguo.servlet.HelloServlet</servlet-class>

    <!--init-param 是初始化参数-->
    <init-param>
        <!--是参数名-->
        <param-name>username</param-name>
        <!--是参数值-->
        <param-value>root</param-value>
    </init-param>

    <!--init-param 是初始化参数-->
    <init-param>
        <!--是参数名-->
        <param-name>url</param-name>
        <!--是参数值-->
        <param-value>jdbc:mysql://localhost:3306/test</param-value>
    </init-param>
</servlet>

<!--servlet-mapping 标签给servlet 程序配置访问地址-->
<servlet-mapping>
  <!--servlet-name 标签的作用是告诉服务器，我当前配置的地址给哪个Servlet 程序使用-->
  <servlet-name>HelloServlet</servlet-name>
  <!--
  url-pattern 标签配置访问地址<br/>
  / 斜杠在服务器解析的时候，表示地址为：http://ip:port/工程路径<br/>
  /hello 表示地址为：http://ip:port/工程路径/hello <br/>
  -->
  <url-pattern>/hello</url-pattern>
</servlet-mapping>
```

Servlet程序中的代码

```java
@Override
public void init(ServletConfig servletConfig) throws ServletException {
    System.out.println("2 init 初始化方法");
// 1、可以获取Servlet 程序的别名servlet-name 的值
    System.out.println("HelloServlet 程序的别名是:" + servletConfig.getServletName());
// 2、获取初始化参数init-param
    System.out.println("初始化参数username 的值是;" + servletConfig.getInitParameter("username"));
    System.out.println("初始化参数url 的值是;" + servletConfig.getInitParameter("url"));
// 3、获取ServletContext 对象
    System.out.println(servletConfig.getServletContext());
}
```
**注意点**

![](AfterSide_imgs/4.jpg)

## ServletContext 类

---


### 什么是ServletContext 类

- ServletContext 是一个接口，它表示Servlet 上下文对象

- 一个web 工程，只有一个ServletContext 对象实例。

- ServletContext 对象是一个域对象。

- ServletContext 是在web 工程部署启动的时候创建。在web 工程停止的时候销毁。

-什么是域对象?

域对象，是可以像Map 一样存取数据的对象，叫域对象。

这里的域指的是存取数据的操作范围，整个web 工程。

![](AfterSide_imgs/5.png)

---


### ServletContext 类的四个作用

- 获取web.xml 中配置的上下文参数context-param

- 获取当前的工程路径，格式: /工程路径

- 获取工程部署后在服务器硬盘上的绝对路径

- 像Map 一样存取数据

ServletContext 演示代码

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

         //获取web.xml中配置上下文的参数context——param
        ServletContext context = getServletConfig().getServletContext();

        String username = context.getInitParameter("username");
        System.out.println("username的名字是："+username);

        //获取当前工程的路径
        System.out.println("当前工程的路径是"+context.getContextPath());

        //获取工程部署后在服务器硬盘上的绝对路径
        /**
         *  /斜杠被服务器解析地址为 http://ip:port/工程名/  映射到idea上的web目录</br>
         */
        System.out.println("工程部署的绝对路径是"+ context.getRealPath("/"));
    }
```
```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        
        //获取servletcontext对象
        ServletContext context = getServletContext();
        context.setAttribute("key1","value");
        System.out.println("context数据域中key1的值是"+context.getAttribute("key1"));
}
```

web.xml里的配置
```xml
<!--context-param 是上下文参数(它属于整个web 工程)-->
<context-param>
    <param-name>username</param-name>
    <param-value>context</param-value>
</context-param>
<!--context-param 是上下文参数(它属于整个web 工程)-->

```

服务器端结果

![](AfterSide_imgs/6.png)
![](AfterSide_imgs/7.png)