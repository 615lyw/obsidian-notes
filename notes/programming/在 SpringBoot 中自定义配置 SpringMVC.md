# 场景

- 在 SpringBoot 自动配置生效的基础上进行自定义
- 关闭 SpringBoot 自动配置

## 配置原理

`WebMvcConfigurer` 是一个接口, 其中提供了大量 default 的空实现方法.

`WebMvcConfigurationSupport` 是 SpringMVC 的默认配置类.

> This is the main class providing the configuration behind the MVC Java config.

`DelegatingWebMvcConfiguration` 会检测所有实现了 `WebMvcConfigurer` 的 beans, 可以在这些 beans 中自定义 `WebMvcConfigurationSupport` 提供的默认配置.

> A subclass of `WebMvcConfigurationSupport`
> detects and delegates to all beans of type `WebMvcConfigurer` allowing them to customize the configuration provided by `WebMvcConfigurationSupport`. This is the class actually imported by `@EnableWebMvc`

```
@Retention(RetentionPolicy.RUNTIME)  
@Target(ElementType.TYPE)  
@Documented  
@Import(DelegatingWebMvcConfiguration.class)  
public @interface EnableWebMvc {  
}
```

`@EnableWebMvc` imports `DelegatingWebMvcConfiguration`

![](https://gitee.com/ryan615/PicGoBed/raw/master/image/20200920170129.png)

- Spring 提供了接口 `WebMvcConfigurer` 用于配置 SpringMVC
- SpringBoot 通过 `WebMvcAutoConfiguration` 中的静态内部类 `WebMvcAutoConfigurationAdapter` 实现了 `WebMvcConfigurer` 中的接口
- 在 `application.yml` 中的 MVC 配置项将会被 SpringBoot 的机制读入, 然后通过 `WebMvcAutoConfigurationAdapter` 去定制初始化

## Enable MVC Configuration

SpringBoot 基于 Java Config 的方式对  SpringMVC  进行配置.

```java
@Configuration
@EnableWebMvc
public class WebConfig {
}
```

## MVC Config API

`WebMvcConfigurer` 接口中定义了大量的空 default 实现.

```java
@Configuration
@EnableWebMvc
public class WebConfig implements WebMvcConfigurer {

    // Implement configuration methods...
}
```

## SpringMVC 常用配置项

```properties
spring.mvc.static-path-pattern=/**  #指定静态资源路径
spring.mvc.view.prefix=             #Spring MVC 视图前缀
spring.mvc.view.suffix=             #Spring MVC 视图后缀
```
