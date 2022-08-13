# SpringBoot

基于 [[Index-SpringFramework]] 拓展，用最小默认配置简化了 Spring 应用的初始化配置，解决了配置繁琐和应用部署繁琐的问题。

# 核心思想：约定优于配置

大部分情况下存在默认配置可以直接使用，如果需要自定义配置简单改下配置文件即可（自定义配置会覆盖默认配置）。

# 起步依赖

spring-boot-starter-web 提供了 SpringMVC 框架所需依赖，spring-boot-autoconfigure 中的默认自动配置类则对 SpringMVC 本身进行配置。

# 自动配置

[[TODO]]

# 自动配置 SpringMVC

```java
@ConditionalOnClass({ Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class })
@ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
public class WebMvcAutoConfiguration
```

当 classpath 下存在 `WebMvcConfigurationSupport` 类时, SpringBoot 的自动配置类不生效。

# 定制 SpringMVC

[[在 SpringBoot 中自定义配置 SpringMVC | 定制 SpringMVC]]

# Spring Configuration Processor

作用：在 application. yml 中给组件的成员变量赋值时会有属性名的提示。

```
<dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-configuration-processor</artifactId>
     <optional>true</optional>
 </dependency>
```

# yml 配置文件语法

[[YAML]]

[[@ConfigurationProperties 注解]]

# 配置文件占位符

通过 `${key}` 的形式来引用同一配置文件中的某个 key 的值，等价于 `变量`。

```yml
file:
  upload:
    basePath: e://springz
    jpgPath: ${file.upload.basePath}/jpg
    pngPath: ${file.upload.basePath}/png
    xmlPath: ${file.upload.basePath}/xml
```

# 配置 SpringMVC 拦截器组件 Interceptor

Interceptor 相关原理和知识详见 [[SpringMVC#Interceptor 拦截器]]。

HandlerInterceptorAdapter 和 HandlerInterceptor 的区别：

类层级关系：

HandlerInterceptor

AsyncHandlerInterceptor

HandlerInterceptorAdapter

自定义 MyInterceptor：

```java
@Component
public class MyInterceptor extends HandlerInterceptorAdapter {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("MyInterceptor....preHandle....调用到了");
        return true;
    }
}
```

注册自定义的 MyInterceptor 并配置拦截范围：

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Autowired
    private MyInterceptor myInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(myInterceptor)
                // ** 表示拦截所有的请求
                .addPathPatterns("/**");
    }
}
```

# 多环境配置

以 yml 配置文件举例。

SpringBoot 的默认配置文件是 `application.yml` . 可以通过 profile 实现多环境配置:  `application-{profile}.yml`, profile 一般为 dev, test, prod.

需要注意的是 `application-{profile}.yml` 配置文件会覆盖默认配置文件 (`application.yml`) 下的同一属性.

在 `application.yml` 中指定以哪个配置文件作为构建基础:

```yaml
spring:
  profiles:
    active: dev
```

# 配置 Druid 数据源

[[Alibaba Druid 数据库连接池]]

# 异步任务

https://blog.didispace.com/spring-boot-learning-2-7-5/

`@Async`
`@EnableAsync`

# 参考

- [知乎-Spring Boot要如何学习？](https://www.zhihu.com/question/53729800)
