# Tomcat

## 什么是 Tomcat？

1. 最初叫 javaWebServer：一个用 Java 语言写的服务器软件
2. Servlet 容器（Servlet 引擎），为 Servlet 提供运行时环境
3. （需要配置 JAVA_HOME 环境变量）

## Tomcat 目录结构

bin：

		startup. sh 和 shutdown. sh

conf：

		Catalina 存储针对每个虚拟主机的 context 配置（默认存放了 localhost 虚拟主机）
		server. xml 配置 Tomcat 服务器（监听端口号，默认 Servlet 和 JSP，`<servlet-name>` 和 `<servlet-mapping>`，默认的 session 超时时间）
		tomcat-users. xml 配置 Tomcat 的用户信息
		web. xml web 应用的默认配置

lib：

		存放 tomcat 依赖的 jar 包

logs：

		Tomcat 日志

temp：

		临时文件

webapps：

		Tomcat 默认 web 应用

work：

		与 JSP 有关

## Tomcat 架构

http 服务器：

1. 接受用户的 http 请求 + 发送 http 响应。即 Tomcat 里的 connector
2. 把文本形式的请求封装成一个对象传给 Servlet 容器

### Tomcat 两个核心组件

从需求看 Tomcat 所需的核心功能（组件）

1. 拿到对象形式的请求和空响应对象，根据请求的 URL 和 Servlet 之间的映射关系，去找对应的 Servlet
2. 如果 Servlet 还未加载，则通过反射加载并调用 init 方法完成初始化；若已加载则到 (3) 【单例】
3. 再调用 Servlet 的 service 方法完成对请求的处理，把 ServletResponse 对象返给 http 服务器，由 http 服务器解析 ServletResponse 对象转变为文本流发给浏览器。

核心：定位 → 加载 → 调用

定位：

url-pattern → servlet-name → servlet-class

加载：

servlet-class（全类名）+ 反射 + 实例化 Servlet

调用：
实例化 Servlet → Method. invoke (obj, varargs)

Tomcat 要实现的两个核心功能：

1. 处理 Socket 连接，负责网络字节流与 request 和 response 对象之间的转换
2. 加载、调用 Servlet 来处理 request 请求

两个核心组件：

connector（对外交流） + container（逻辑处理）

### Tomcat 组成结构

```
<Service>
  <Connector>
  <Connector>
  <Engine>
    <Host>
      <Context/>
    </Host>
    <Host>
      <Context/>
    </Host>
  </Engine>
</Service>
```

网络协议不只是 HTTP，还有很多其他协议，Tomcat 支持处理多种网络协议请求，一个 Connector 对应处理一种协议，一个 Engine 可以对应多个 Connector。

单独的 Connector 或 Engine 是无法完成对网络请求的处理的，需要把两者组装起来作为一个整体构成一个 `<Service>` 对外提供服务。

## 1.4 如何在 Tomact 里部署一个 web 应用？

### 1.4.1 如何理解一个 Web 应用下有很多个 Servlet ？

就像手机上面的应用程序，有很多个模块页面，你可以理解为跳转到一个模块页面就有一个 Servlet 为你服务。

### 1.4.2 三种部署方式

1. 在 webapps 目录中新建一个应用目录，目录名即应用名
2. conf/server. xml 文件中，host 节点下新增一个 Context 节点
3. conf/Catalina/localhost 下面新建一个应用名. xml 文件

### 1.4.3 应用的目录层次规定

（与 Tomcat 实现有关）

MyWebApp（应用名）

|---- 静态资源

|---- WEB-INF（屏蔽客户端的直接访问）

​	|---- classes

​	|----lib

​	|----web. xml：应用的配置文件，主要用来配置 Servlet（做 URL → Servlet 的映射）

### 1.4.4 web 应用配置

web. xml 是 web 应用的配置文件

全局默认配置：tomcat/conf/web. xml
局部配置：每个 web 应用下 WEB-INF/web. xml 文件

如何在某个应用的 web. xml 中配置 Servlet 映射信息？

```xml
<servlet>
    <servlet-name>firstServlet</servlet-name>
    <!--用到反射技术，Class.forName(String 全类名)-->
    <servlet-class>com.cskaoyan.firstServlet</servlet-class>
 </servlet>

<servlet-mapping>
    <servlet-name>firstServlet</servlet-name>
    <url-pattern>/first</url-pattern>
</servlet-mapping>
```

## 1.5 Tomcat 请求处理流程是什么？

http://localhost:8080/appname/servlet

/appname：应用的 path

/servlet：url-pattern

1. 浏览器构建请求报文发送给 Tomcat 服务器
2. 在 conf/server. xml 中配置的 connector 监听 8080 端口，接受到请求报文，为了方便处理文本流构建请求对象和空的响应对象传给 Engine
3. 在 conf/server. xml 中配置的 Engine 接受请求和响应对象传给 defaultHost
4. host 根据 appname 去 webapps 里找传给 appname 这个应用
5. **应用根据自己的 web. xml 配置决定调用哪个 Servlet 执行**
