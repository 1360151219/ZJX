

## Cookie

### 什么是Cookie?

- Cookie翻译过来是饼干的意思。
  
- Cookie是服务器通知客户端保存键值对的一种技术。
  
- 客户端有了Cookie后，每次请求都发送给服务器。
  
- 每个Cookie的大小不能超过4kb

---

### 如何创建Cookie
创建一个Cookie可以通过`respone.addcookie(cookie)`来创建

我们可以看看一个Cookie的创建过程如下:

![](ServerSide_imgs/13.jpg)

---

### 服务器如何获取Cookie

服务器获取客户端的Cookie 只需要一行代码：`request.getCookies()`

其中`request.getCookies()`获取的是存储在客户端的Cookie数组，即是**全部**Cookie

其创建过程如下:

![](ServerSide_imgs/14.jpg)

---

### Cookie 值的修改

- 方案一：

1、先创建一个要修改的同名（指的就是key）的Cookie 对象

2、在构造器，同时赋于**新的**Cookie 值。
```java
Cookie cookie = new Cookie("key1","newValue1");
```
3、调用`response.addCookie(Cookie)`;

- 方案二：

1、先查找到需要修改的Cookie 对象

2、调用`setValue()`方法赋于新的Cookie 值。
```java
cookie.setValue("newValue2");
```
3、调用`response.addCookie()`通知客户端保存修改

>区别: 

>- 方案一是创建一个新的同名Cookie来覆盖掉原来Cookie

>- 方案二是直接修改需要修改的Cookie值

---

### 浏览器查看Cookie

- 谷歌浏览器查看Cookie

![](ServerSide_imgs/15.jpg)

- 火狐浏览器查看Cookie

![](ServerSide_imgs/16.jpg)

---

### Cookie的生命控制

- Cookie的生命控制指的是如何管理Cookie什么时候被销毁（删除）

- 我们可以通过`setMaxAge()`来设置Cookie的生命控制


`setMaxAge(正数)`，表示在指定的秒数后过期

`setMaxAge(负数)`，表示浏览器一关，Cookie就会被删除（默认值是-1）

`setMaxAge(0)`，表示马上删除Cookie

- 其中，Cookie的生命周期可以在浏览器里查看

![](ServerSide_imgs/17.png)

---

### Cookie有效路径Path的设置

- Cookie的path属性可以有效的过滤哪些Cookie可以发送给服务器。哪些不发。
  
- path属性是通过请求的地址来进行有效的过滤。

- `getContextPath()` ===>>>> 得到工程路径

![](ServerSide_imgs/18.png)

- 代码如下:
```java
cookie.setPath( req.getContextPath() + "/abc" ); 
```



---

### Cookie验证登录的一个简单流程

![](ServerSide_imgs/19.jpg)

>## Session会话

---

### 什么是Session 会话?

- Session 就一个接口（HttpSession）。
  
- Session 就是会话。它是用来维护一个客户端和服务器之间关联的一种技术。
  
- 每个客户端都有自己的一个Session会话。
  
- Session会话中，我们经常用来保存用户登录之后的信息。

---

### 如何创建Session和获取(id号,是否为新)

如何创建和获取Session。它们的API是一样的。

- `request.getSession()`

第一次调用是：创建Session会话
  
之后调用都是：获取前面创建好的Session会话对象。（单例模式？）

- `isNew()`; 判断到底是不是刚创建出来的（新的）

**true**表示刚创建

**false**表示获取之前创建

- `getId()` 得到Session 的会话id 值。

每个会话都有一个身份证号。也就是ID 值。而且这个ID 是唯一的。

---

### Session 域数据的存取

`setAttribute()`设置当前Session的域数据

`getAttribute()`获取当前Session的域数据

```java
req.getSession().setAttribute("key1", "value1");
req.getSession().getAttribute("key1");
```

### Session 生命周期控制

- `session.setMaxInactiveInterval(int interval)`

设置Session 的超时时间（以秒为单位），超过指定的时长，Session就会被销毁。

值为**正数**的时候，设定Session 的超时时长。

**负数**表示永不超时（极少使用）

- `getMaxInactiveInterval()`获取Session 的超时时间
  
- `session.invalidate()` 让当前Session 会话马上超时无效。

!>Session 默认的超时时间长为30 分钟。

---

### 浏览器和Session关联的内幕

Session 技术，底层其实是基于Cookie 技术来实现的。

![](ServerSide_imgs/20.jpg)