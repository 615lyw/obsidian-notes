---
date created: 2022-12-30, 23:13:54
date modified: 2022-12-31, 09:49:35
---

背景：在项目中，为了方便其他微服务进行 [[OpenFeign]] 调用，有一个 Feign 接口类，其中定义了微服务 Controller 中的所有接口，且 Controller 实现了该 Feign 接口。

```java
@FeignClient("user-microservice")
public interface User {
    @GetMapping("/user/getUserInfo")
    UserInfo getUserInfo(@RequestParam("userId") Long userId);
}

@RestController
public class UserController implements User {
    @Override
    public UserInfo getUserInfo() {
        // ...
    }
}
```

# 歧路 1

在 Feign 接口中定义完新接口后，Controller Override 实现，但没有标注 `@GetMapping` 方法注解和 `@RequestParam` 参数注解，结果实际调用时报 404 接口未找到。当再加上 `@GetMapping` 方法注解和 `@RequestParam` 参数注解时，又没有报 404 错误了。故当时以为是注解问题。

认知：当时以为子类会继承了父类方法上的注解以及参数注解，所以并没有在 Controller 上重新标注注解。

疑惑：子类是否会继承父类方法上的注解吗？[[注解继承]]

行为：后续写 Controller 接口时都把 Feign 接口上的方法注解和参数注解都加上。

其实仍有可能出现 404 问题，因为 404 是负载均衡导致的，本地启的服务 A 调用下游服务 B （本地启动、写了新接口的服务）时被负载均衡至服务器上的服务 B 了，服务器上代码没有更新，所以会产生 404。（后续有次注解都写了，仍然存在 404 时发现了是负载均衡问题，但没有联想到更新这块知识）

# 歧路 2

又发现同事说方法上的注解其实不用，只需要方法参数上的注解就不会报 404 了。自己写了个 SingleSpringBoot 测 Controller 到底能否继承接口上的注解，发现 Controller 只需要一个 `@Override` 就行。证明接口方法和参数上的注解都能继承。

疑惑：子类明明不能继承接口上的注解，这是怎么回事，猜想是 SpringWebMVC 框架解析原因，可能递归查询父类了。[[Spring 注解派生和 AnnotationUtils]]

认知：Spring 在建立 URL 和 Controller 接口映射关系时使用 AnnotationUtils 来处理注解，AnnotationUtils 会递归去找父类上是否存在注解，故 Feign 接口上写了 `@RequestMapping` 则子类 Controller 上不用写。

# 歧路 3

又去公司项目试验（[[OpenFeign#方便调试]]），又报错（[[Spring Cloud#兼容性]]），发现 Controller 方法参数上的注解不能少，但为什么自己的 SingleSpringBoot 没问题。

发现是 SpringBoot 版本的原因，自己的 SingleSpringBoot 版本为 2.1.0，公司的是 2.0.3。根据版本列表依次试验 2.0.3 到 2.1.0 之间的版本，发现就 2.1.0 不用写方法参数注解。

SpringBoot 2.1.0 对应 SpringFramework (包含了 WebMVC) 5.1 版本，在 [官方 Release](https://github.com/spring-projects/spring-framework/wiki/What%27s-New-in-Spring-Framework-5.x#whats-new-in-version-51) 上找到：

![[Pasted image 20221231094800.png]]
