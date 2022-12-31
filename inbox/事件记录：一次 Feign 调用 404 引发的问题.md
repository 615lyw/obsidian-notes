---
date created: 2022-12-30, 23:13:54
date modified: 2022-12-30, 23:16:18
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

在 Feign 接口中定义完新接口后，Controller Override 实现，但没有标注 `@GetMapping` 方法注解和 `@RequestParam` 参数注解，结果实际调用时报 404 接口未找到。当再加上 `@GetMapping` 方法注解和 `@RequestParam` 参数注解时，没有报 404 错误了。故当时以为是注解问题。

认知：当时以为子类会继承了父类方法上的注解以及参数注解，所以并没有在 Controller 上重新标注注解。

行为：后续写 Controller 接口时都把 Feign 接口上的方法注解和参数注解都加上。

疑惑：子类是否会继承父类方法上的注解吗？[[注解继承]]



正确行为：像以前一样常规定义 Controller 中的接口，只是多加一个 `@Override` 注解而已。
