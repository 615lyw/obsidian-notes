# 背景

Web application 两种技术栈：

- Servlet
- Reactive

# Spring Web MVC

基于 Springframework + Servlet API 的一种 Web 开发框架。

# DispatcherServlet

`dispatcherServlet` 同任一 `Servlet` 一样，需要遵循 Servlet 规范：任何一个 Servlet 都需要在 `web.xml` 中被声明（declared）并且建立和 URL-Pattern 之间的映射（mapped）关系。

如：

```xml
<servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath: resources/application.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>

<servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
    <url-pattern>/app/*</url-pattern>
</servlet-mapping>
```

由于 `dispatcherServlet` 的真正工作只是把请求处理委派给其他的组件，故只需要在 `web.xml` 中注册一个 `dispatcherServlet` 即可，其他组件均可以通过 Springframework 机制来实现依赖注入。

上面这种注册方式依赖于外部 Servlet 容器的生命周期。Spring Boot 则通过  Spring configuration 和内嵌的 Servlet 容器实现所有组件（Servlet、Filter 等）的注册。

# Special Bean Types

前端请求 ---> 基础设施 bean ---> 业务代码.

- HandlerMapping
- HandlerMapping
- HandlerExceptionResolver
- ViewResolver
- LocaleResolver
- ThemeResolver
- MultipartResolver
- FlashMapResolver

# MVC 架构

![](https://gitee.com/ryan615/PicGoBed/raw/master/image/20200920144003.png)

# Spring MVC 流程图

<img src="https://gitee.com/ryan615/PicGoBed/raw/master/image/20200519210046.png"/>

# Spring MVC 具体流程

DispatcherServlet 是 SpringMVC 的核心。

处理器映射：

SpringMVC 在启动阶段将 @RequestMapping 所配置的 URL 和对应 handler 保存到 HandlerMapping 中，请求到来时，通过拦截请求信息进行匹配找到对应的 handler，将 handler 和其拦截器保存到 HandlerExecution Chain 对象中，返回给 DispatcherServlet 来执行。

# 基础注解

```java
@Controller

@Service

@Repository
```

# Controller 中的方法获取参数

## 无注解

- GET 请求
- `public String noAnnotation(Integer intVal, Long longVal, String str)`
- 要求前端参数名与方法参数名一致
- 默认规则下接受前端少传参数

## 使用＠RequestParam () 获取参数

- `@RequestParam(value = "str_val", required = false)`
- `public String requestParam(@RequestParam("str_val") String strVal) `
- 作用：建立前端参数名与方法参数名之间的映射关系
- value 为前端参数名
- 默认规则下不接受前端少传参数，如需要，配置 `required = false`

## 传数组（暂略）

## 传 JSON

- POST 请求

- `@RequestBody`

- `public String insert (@RequestBody User user)`
- JSON 请求体中的 key 与 User 类之间的属性名要保持一致
- 还可以封装为 Map 或 List：`public String foobar(@RequestBody List<User> users)`

## 传 URL（REST）

# Controller 中的方法返回值

// SpringMVC 会将返回结果转化为 JSON 格式
@ResponseBody+类或方法



// 等价于@Controller+@ResponseBody
@RestController+类





注解详解：
`@RequestMapping(“url”)`
- 作用：请求路径 → 控制器（或其方法）
- 可以加在类上或方法上，在类上作用为窄化请求
- `@GetMapping` 和 `@PostMapping` ：限定请求方法

# 解决 post 请求参数中文乱码问题

乱码原因：get 请求请求参数放在 URL 中，post 请求参数放在正文中，而 Tomcat 的缺陷在于对 http 请求正文的编码不是 utf-8，故只有 post 请求才有中文乱码问题。

解决方案：

web. xml 中配置 CharacterEncodingFilter，url-pattern 为 /*，通过 init-param 完成 encoding=utf-8 和 forceEncoding=true 的初始化赋值。（init-param 类似 `<property>` ）

```
<filter>
    <filter-name>characterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>utf-8</param-value>
    </init-param>
    <init-param>
        <param-name>forceEncoding</param-name>
        <param-value>true</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>characterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

# converter 类型转换器

作用：前端传来的参数是字符串类型，通过 converter 转换为其他数据类型的值

Spring 提供了很多字符串转换为其他类型的 converter，如何取得该 converter？

```java
@Autowired
ConfigurableConversionService conversionService;

@RequestMapping("/output/converters")
public String output() {
    System.out.println("conversionService = " + conversionService);
    return "";
}
```

案例：完成 String 类型的 "2020-05-20" → Date 类型的日期

## 法一：使用 SpringMVC 提供的 converter

在成员变量上使用

```java
@Data
public class User {
    @DateTimeFormat(pattern = "yyy-MM-dd")
    Date birthday;
}
```

在 handler 形参上使用

```java
@RequestMapping("submit/date")
public String submitDate(@DateTimeFormat(pattern="yyy-MM-dd") Date birthday) {
    return "";
}
```

## 法二：自定义 converter

1. 实现 Convert 接口

    Converter<源类型, 目标类型>

```java
@Component
public class String2DateConverter implements Converter<String, Date> {
    // s 为前端传来的参数
    @Override
    public Date convert(String s) {
        SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");
        try {
            return dateFormat.parse(s);
        } catch (ParseException e) {
            e.printStackTrace();
        }
        return null;
    }
}
```

2. 注册自定义转换器组件并注入 FormattingConversionService

```
<bean id="formattingConversionService" class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
    <property name="converters">
        <set>
            <ref bean="string2DateConverter"/>
        </set>
    </property>
</bean>
```

3. 通知 mvc 注解驱动器

```
<mvc:annotation-driven conversion-service="formattingConversionService"/>
```

# 文件上传

1. 引入 maven 依赖

```
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.4</version>
</dependency>
```

2. 通过 `<bean>` 注册相关组件：CommonsMultipartResolver

注意 id 为固定值 multipartResolver

```
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver"/>

```

3. 前端写法

```html
<input type="file" name="myfile"> 其中 name 表示前端参数名
```

4. 后端写法

- 形参：MultipartFile myFile（ 需与前端参数名 name 的值保持一致）

- myFile. transferTo (file) 完成文件的存储

- myFile. getOriginalFilename () 获取用户实际上传的文件名

```java
public String register(MultipartFile myFile) {
    String fileName = myFile.getOriginalFilename();
    File file = new File("/Users/ryan/Pictures/project-static-resources", fileName);
    try {
        myFile.transferTo(file);
    } catch (IOException e) {
        e.printStackTrace();
    }
    return "/WEB-INF/success.jsp";
}
```

# cookie 和 session

session 用法同 JavaEE 阶段学的内容。

```java
public String foo(HttpSession session) {
    session.setAttribute("key", value);
    session.getAttribute("key");
    return "/WEB-INF/success.jsp";
}
```

cookie

# RESTful API 风格

以前 url-pattern 为：queryUserById? id=5

RESTful 风格：

- 请求 URL 设计风格：操作主体/行为/参数，比如：/liuyiwei/article/details/123
- 返回值为 json 数据



比如仿 csdn 的 url-pattern 写法：

通过 @PathVariable 获取 {username} 变量值赋值给形参

```java
@RequestMapping("/{username}/article/details/{articleId}")
public String foo(@PathVariable("{username}") String username, @PathVariable("{articleId}") int id) {
    return "/WEB-INF/success.jsp";
}
```

# 静态资源的访问

之前是由默认 Servlet 处理，如今用 dispatchServlet 替代了默认 Servlet，对静态资源的访问被 dispatchServlet 当做 URL 去找 handler 了，而又没有对应处理静态资源的 handler，故会报错。

## 法一：使用默认 Servlet 处理

```

```

`<servlet-mapping>` 使用 default 设置为 \*. jsp、\*. css

## 法二：提供默认的 handler

## 法三：静态资源映射【推荐】

把前端访问静态资源的 URL 映射到一个路径，即 url mapping → 真实路径 location

```
<mvc:resources mapping="/pic/**" location="file:/Users/ryan/Pictures/project-static-resources/"/>
```

** 的含义：允许多级 url

例子：/pic/abc/abc. jpg → location 下的 /abc/abc. jpg

映射规则：







注意 location 里写的路径最后一定要有 /：

- /abc/ 表示 abc 目录
- /abc 表示 abc 文件

location 三种路径写法：

# 以 Json 格式接受或返回数据

添加 jackson-databind 依赖：

```
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.10.4</version>
</dependency>
```

<img src="https://gitee.com/ryan615/PicGoBed/raw/master/image/20200609085150.png"/>

##  后端接受 json

将 json 类型的参数封装为 User 类型的 javabean

```java
public String foobar(@RequestBody User user)
```

根据 json 格式的不同还可封装为对应的多种类型：Map、List 等

```java
public String foobar(@RequestBody List<User> users)
```

## 后端返回 json

可以定义通用返回模型：

```java
/**
 * 通用的返回值的模型
 */
@Data
public class BaseRespVo {
    int retCode;
    String msg;
  	Object data;
}

```

- 类上写 @ResponseBody：表示其下所有的方法返回均为 json 格式
- @RestController = @ResponseBody + @Controller
- 方法上写 @ResponseBody：该 handler 返回为 json 格式

```java
@RequestMapping("login")
@ResponseBody
public Object login(String username, String password) {
    BaseRespVo baseRespVo = userService.login(username,password);
    return baseRespVo;
}
```

前端写法：

通过 postman 构造 json 请求

1. header 新增 Content-Type=application/json
2. body 写入 json 数据
3. 使用 post 方法

# Interceptor 拦截器

## 基本概念

作用：类似 filter 对请求和响应进行预处理和后处理。

常见场景：日志记录、权限检查、性能监控等多个 handler 都需求添加的功能。

## 流程图

HandlerInterceptor 所处位置：

DispatchServlet → HandlerMapping → **HandlerInterceptor** → HandlerAdapter → Handler → DispatchServlet

<img src="https://gitee.com/ryan615/PicGoBed/raw/master/image/20200519210046.png"/>

## 代码写法

实现 HandlerInterceptor 接口并通过 @Component 注册组件

```java
@Component
public class CustomInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        return false;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {

    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {

    }
}
```

## 配置

配置一个拦截器：

```
<mvc:interceptors>
	<mvc:interceptor>
    <!--法一：通过引用 bean id 的方式完成注册-->
    <ref bean="customInterceptor"/>
  </mvc:interceptor>
    
  <mvc:interceptor>
    <!--法二：单独注册自定义拦截器-->
    <bean class="com.cskaoyan.interceptor.CustomInterceptor"/>
  </mvc:interceptor>
</mvc:interceptors>
```

或配置多个拦截器：

```
<mvc:interceptors>
  <mvc:interceptor>
    <ref bean="customInterceptor1"/>
  </mvc:interceptor>
  <mvc:interceptor>
    <ref bean="customInterceptor2"/>
  </mvc:interceptor>
  <mvc:interceptor>
    <ref bean="customInterceptor3"/>
  </mvc:interceptor>
</mvc:interceptors>
```

## 拦截器作用范围

从流程图可以看出，Interceptor 处于 HandlerMapping 之后，故只有前端访问的 url-pattern 有对应的 handler 进行处理的这部分请求 HandlerMapping 才会转发给 Interceptor 进行拦截。

Interceptor 不是对前端任意请求都会拦截，只有已实现 url-pattern → handler 之间映射的请求才会被拦截。

>   `` `txt
>   Called after HandlerMapping determined an appropriate handler object
>   `` `



综上所述：Interceptor 只对要进入 handler 方法的全部或部分请求进行拦截。



默认作用范围：Interceptor 对进入 handler 方法的全部请求进行拦截。

通过 `<mvc:mapping path=>` 对部分请求进行拦截。

```
<!--path 里的值表明对哪一部分 url 进行拦截-->
<mvc:interceptors>
	<mvc:interceptor>
		<!--对/hello/下的所有子 url 进行拦截-->
      <mvc:mapping path="/hello/**"/>
      <ref bean="customInterceptor1"/>
  </mvc:interceptor>
    
  <mvc:interceptor>
    <!--作用范围：DispatcherServlet 的全部请求-->
    <ref bean="customInterceptor2"/>
  </mvc:interceptor>
    
  <mvc:interceptor>
    <!--对/hello2/下的所有子 url 进行拦截-->
    <mvc:mapping path="/hello2/**"/>
    <ref bean="customInterceptor3"/>
  </mvc:interceptor>
</mvc:interceptors>
```

## 拦截案例

拦截指定 URL：

<img src="https://gitee.com/ryan615/PicGoBed/raw/master/image/20200520145650.png"/>



其他不满足拦截 URL 的执行情况：

<img src="https://gitee.com/ryan615/PicGoBed/raw/master/image/20200520145806.png"/>

## 3 个方法

执行顺序：preHandle → handler → postHandle → afterCompletion。



**preHandle**：

常用于拦截请求或对请求做预处理

返回值为 boolean 类型，决定是否往下继续执行流程

- 如果返回值为 false 不能继续往下执行

- 如果返回值为 true，则一定可以执行到对应的 afterCompletion



**postHandle：**

如果 handler 返回的是 ModelAndView ，可在此处进行后处理再返回给 DispatchServlet。



**afterCompletion**：

类似 finally，即使 handler 发生了异常也会执行到此方法，故可以把释放资源的操作放于此处

## 多个 Interceptor 执行顺序

按照在 application. xml 中的注册顺序来排序，谁写在前面就先执行谁。

## 基本模型

<img src="https://gitee.com/ryan615/PicGoBed/raw/master/image/20200520144459.png"/>



上图 prehandle\posthandle\afterCompletion 三部分的执行顺序为：12\21\21



案例：

现在有三个 interceptor 分别叫 interceptor1、interceptor2、interceptor3 如果其中一个的 prehandle 方法的返回值为 false，执行顺序为：

1、 interceptor1 的返回值为 false，其余两个为 true：1\ \

2、 interceptor2 的返回值为 false，其余两个为 true：12\ \1

3、 interceptor3 的返回值为 false，其余两个为 true：123\ \21

## 规律

1. 模型中一旦某个 prehandle 方法返回值为 false，则停止执行后续流程（该 prehandle 还是执行完了的）
2. 如果某 prehandle 返回值为 true，一定可以执行到其对应的 afterCompletion，posthandle 不一定执行（因为可能会因为后面的 Interceptor 的 prehandle 为 false；即 prehandle 与 afterCompletion 的关系是绑定的）
3. 所有的 posthandle 要能执行取决于最后一个 prehandle 为 true

## 如果在执行 handler 过程中出现异常

规律：

1. 遇异常则停（其后所有 posthandle 都执行不到）
2. 依旧满足 prehandle 如果返回值为 true，一定可以执行到对应的 afterCompletion

# 异常处理

基础知识：会自定义异常。

原则：不把异常信息暴露给用户，即不抛给前端页面显示。

## 整体流程

dispatcherServlet 把 handler、service、dao 向上抛出的异常交给 handleExceptionResolver 来进行统一的异常处理。

<img src="https://gitee.com/ryan615/PicGoBed/raw/master/image/20200520150436.png"/>

## 需要返回 ModelAndView 时的异常处理写法

1. 实现 HandlerExceptionResolver 接口
2. 通过 @Component 注册组件
3. 形参 Object handler 为发生异常的 handler
4. 形参 Exception e 为发生异常的 handler 抛出的异常

```java
@Component
public class CustomExceptionResolver implements HandlerExceptionResolver {
    @Override
    public ModelAndView resolveException(HttpServletRequest httpServletRequest, 
    HttpServletResponse httpServletResponse, Object handler, Exception e) {
        ModelAndView modelAndView = new ModelAndView();
        // 根据异常的类型的不同，可以做个性化的分发
        // SensitiveWordException 和 MaxSizeException 为自定义异常
        if (e instanceof SensitiveWordException) {
            SensitiveWordException exception = (SensitiveWordException) e;
            String word = exception.getWord();
            String message = exception.getMessage();
            modelAndView.setViewName("/WEB-INF/view/sensitive.jsp");
            modelAndView.addObject("name",word);
            modelAndView.addObject("message",message);
        } else if (e instanceof MaxSizeException) {
            modelAndView.setViewName("/WEB-INF/view/maxSize.jsp");
            String message = ((MaxSizeException) e).getMessage();
            modelAndView.addObject("message",message);
        } else {
            modelAndView.setViewName("/WEB-INF/view/exception.jsp");
        }
        return modelAndView;
    }
}
```

## 需要返回  json 格式时异常处理写法

1. 类上加 @ControllerAdvice 表明这是一个异常处理组件
2. 方法上加 @ExceptionHandler ({异常类型. class}) → 当发生对应异常时，由该异常处理 handler 来处理
3. handler 形参传入其所处理的异常类型
4. 以 json 格式返回：@RestControllerAdvice = @ControllerAdvice + @ResponseBody

```java
@RestControllerAdvice
public class CustomExceptionController {
    /**
     * ExceptionHandler 注解中的 value 为 exception 的 class 数组 → 当发生对应的异常时，由该方法处理
     */
    @ExceptionHandler(SensitiveWordException.class)
    public BaseRespVo resolveSensiveExceptionHandler(SensitiveWordException e){
        BaseRespVo baseRespVo = new BaseRespVo();
        baseRespVo.setErrmsg(e.getMessage());
        baseRespVo.setStatus(500);
        return baseRespVo;
    }

    @ExceptionHandler({MaxSizeException.class, NoSuchAlgorithmException.class})
    public BaseRespVo resolveMaxSizeExceptionHandler(MaxSizeException maxSizeException){
        return new BaseRespVo();
    }
}
```

# i18n 国际化与本地化

实现方式：MessageSource + LocaleResolver

原理：

# JdbcTemplate

引入目的：减少 JDBC 样板代码，通过 JdbcTemplate 只需两行代码即可完成 CURD。

## 环境配置

1. JdbcTemplate 对象需要一个 DataSource：`new JdbcTemplate(DataSource datasource)`
2. 通过 druid 来提供所需 DataSource
3. DataSource 需要相应的数据库驱动：mysql-connector-java

```
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.2.4.RELEASE</version>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.47</version>
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.22</version>
</dependency>
```

## 写法

1. 在 application. xml 文件中通过 `<bean>` 标签完成 JdbcTemplate 组件以及所需的 DataSource 组件的实例化
2. 通过 `<property>` 子标签完成依赖注入

```
<!--druidDatasource-->
<bean id="datasource" class="com.alibaba.druid.pool.DruidDataSource">
    <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql://localhost:3306/j20_db"/>
    <property name="username" value="root"/>
    <property name="password" value="123456"/>
</bean>
<!--jdbcTemplate-->
<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
    <property name="dataSource" ref="datasource"/>
</bean>
```

2. 在 Dao 层通过注解获得  JdbcTemplate 组件

```java
@Autowired
JdbcTemplate jdbcTemplate;
```

3. 使用 JdbcTemplate 对象完成 CURD

例如：queryForObejct (sql, 返回值类型, sql 语句所需参数)

```java
public boolean userLogin(String username, String password) {
    String sql = "select id from day08_user_t where username = ? and password = ?";
    try {
        Integer id = jdbcTemplate.queryForObject(sql, Integer.class, username, password);
    } catch (EmptyResultDataAccessException e) {
        return false;
    }
    return true;
}
```

注意：`queryForObject` 没有查询到结果时，会抛出异常 `EmptyResultDataAccessException`，而不是以为的返回 null。
