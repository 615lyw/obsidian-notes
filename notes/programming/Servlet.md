# 3.3 发送文本数据到浏览器乱码问题

为什么出现乱码？

乱码是因为通信双方编解码所用字符集不一致而导致的。



浏览器发送数据和解析数据所依据的编码格式：

<head>
    <meta charset="UTF-8">
</head>



由于 Tomcat 存在 http 报文 body 部分的编码格式不是 utf-8 的缺陷，故**需要在处理请求或响应报文 body 数据前**手动设置编码格式：

```java
response.setCharacterEncoding(String charset)
```

但上述设置还不够，因为浏览器不知道 Tomcat 发过来的响应报文的编码格式，故依旧会出现乱码。

故需要告知浏览器响应体的编码格式：

```java
response.setHeader("content-type", "text/html;charset=utf-8");
response.setCharacterEncoding("utf-8")
```

有直接一行代码就解决乱码问题的 API，其实现了上面两步：

```java
response.setContentType("text/html;charset=utf-8")
```

# 3.4 发送二进制数据到浏览器

情况一：二进制数据在 web 根目录下，此时浏览器可以直接访问到

情况二：二进制数据在 WEB-INF 目录下，该目录对浏览器屏蔽，故只能通过输出流至响应体 body 的形式来实现发送二进制数据到浏览器



>   By default the classes in the `java.io` package always resolve relative pathnames against the current user directory. This directory is named by the system property `user.dir`, and is typically the directory in which the Java virtual machine was invoked.

故 new File(String relativePath) 此时的相对路径是相对于 JVM 的启动路径，即 main 程序入口。

```java
// 显示 /Users/ryan/backend/apache-tomcat-8.5.54/bin
System.getProperty("user.dir")
```

（在 bootstrap.jar 中有 main 方法）



问题：如何获取 WEB-INF 目录下的文件所在路径？

```java
String path = getServletContext().getRealPath("WEB-INF/1.jpg");
```

# 3.5 定时刷新

# 3.6 重定向

 什么是 Servlet？

Servlet = Server + applet (a simple computer program designed to run from a webpage)

运行在 Java 服务器端的小程序，负责处理客户的请求并生成对应的响应。



一个 Servlet 程序片段没有 main 方法，故只能部署在 java 服务器上后才能运行。



 如何实现一个 Servlet？

实现 Servlet 接口，因为有两个（抽象）子类：GenericServlet 和 HttpServlet，故继承这两个类再实现其抽象方法即可。

继承 GenericServlet

![image-20200419084234691](/Users/ryan/Library/Application Support/typora-user-images/image-20200419084234691.png)

![image-20200419084312635](/Users/ryan/Library/Application Support/typora-user-images/image-20200419084312635.png)



继承 HttpServlet (是 GenericServlet 的抽象子类)

![image-20200419084407840](/Users/ryan/Library/Application Support/typora-user-images/image-20200419084407840.png)

![image-20200419084745068](/Users/ryan/Library/Application Support/typora-user-images/image-20200419084745068.png)

# 3.2.1 为什么继承 HttpServlet 不用实现 service 方法？

因为在 HttpServlet 类中已经实现了 service 方法。



为什么这么设计？

因为客户端主要向服务端发 get 或 post 请求，为了针对性的处理两种请求方法，我们就要自己写请求方法判断逻辑，而 HttpServlet 类中已经帮我们实现了这一步，使程序员可以更专注于业务逻辑。



![image-20200419085312147](/Users/ryan/Library/Application Support/typora-user-images/image-20200419085312147.png)





 3.3 Servlet 生命周期函数

Servlet 在不同的生命周期会被 Servlet 引擎调用其相应的生命周期函数，有以下三个生命周期函数：

1. init() ：当 Servlet 首次被创建时被调用，之后内存中始终只有一个 Servlet，称单例
2. service()：每一次对该 Servlet 请求时会被调用
3. destroy：当 Tomcat 停止运行或 Servlet 所在应用被关闭（通过 Tomcat 控制面板取消应用部署）

Servlet 的生命周期函数不是 Servlet 引擎中有关 Servlet 的具体实现，而是在其生命周期阶段在具体实现中调用这些函数，目的是提供给应用程序员让其重写方法以实现在对应的生命周期去完成一定的业务逻辑。

故可以推测出源码中生命周期函数的方法体为空，当子类重写方法后，this.init() 等会调用到子类的实现。（JavaSE 知识）



补充：

init() 默认是在 Servlet 首次被创建时被调用，通过将 load-on-startup 设置为非负数，可以实现当前 Servlet 的 init() 方法随着其所在应用的加载而执行。

两种设置方式：

1. 注解：@WebServlet( load-on-startup = 1)
2. 在 web.xml 中配置

<servlet>

​		<load-on-startup>1</load-on-startup>

</servlet>





 3.4 url-pattern

>   ```
>   String getContextPath()
>   ```
>
>   Returns the context path of the web application.
>
>   The context path is the portion of the request URI that is used to select the context of the request. The context path always comes first in a request URI. **If this context is the "root" context** rooted at the base of the Web server's URL name space, **this path will be an empty string**. Otherwise, if the context is not rooted at the root of the server's name space, the path starts with a / character but does not end with a / character.
>
>   -   **Returns:**
>
>       The context path of the web application, or "" for the root context



如果 Application context 设置为 / ，则 URL 写成：http://localhost:8080/servlet222 即可。

可以理解为：/""/servlet222 简化为：/servlet222

![image-20200420224857084](/Users/ryan/Library/Application Support/typora-user-images/image-20200420224857084.png)



需要注意的 url-pattern 细节：

1. **应用的 path 和 Servlet 的 url-pattern 的写法必须以 / 开头**
2. 一个 Servlet 可以设置多个 url-pattern
3. 多个 Servlet 不能设置为同一个 url-pattern
4. Servlet 的 url-pattern 的写法必须以 / 开头或者是 *.任意后缀名

# 3.4.1 url-pattern 优先级

写 / 和写 \*.xx 时，谁的访问优先级更高？



>   对于如下的一些映射关系：
>   Servlet1 映射到 /abc/\*
>   Servlet2 映射到 /\*
>   Servlet3 映射到 /abc
>   Servlet4 映射到 \*.do
>
>   问题：
>   当请求 URL 为 “/abc/a.html”，“/abc/\*” 和 “/\*” 都匹配，哪个 servlet 响应？
>   	Servlet 引擎将调用 Servlet1。
>   当请求 URL 为“/abc”时，“/abc”   "/\*" 都匹配，哪个 servlet 响应？
>   	Servlet引擎将调用Servlet3。
>   当请求 URL 为“/abc/a.do”时，“/abc/\*” 和 “\*.do” 和 "/\*" 都匹配，哪个servlet响应？
>   	Servlet 引擎将调用 Servlet1。
>   当请求 URL 为 “/a.do” 时，“/\*” 和 “\*.do” 都匹配，哪个 servlet 响应？
>   	Servlet 引擎将调用 Servlet2。
>   当请求 URL 为 “/xxx/yyy/a.do” 时，“/\*” 和 “\*.do” 都匹配，哪个 servlet 响应？
>   	Servlet 引擎将调用 Servlet2。



故规律为：

1. / 的优先级高于 \*
2. 都以 / 开头，则精准匹配程度越高，优先级最高

# 3.4.2 特殊的 url-pattern

因为 Servlet 的 url-pattern 的写法可以是 *.后缀，故可写为：@WebServlet("\*.html")，那么当我访问 1.html 时，到底访问的是 1.html 静态资源文件还是这个 Servlet 呢？（\* 表示通配符）
