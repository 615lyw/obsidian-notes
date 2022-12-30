---
date created: 2022-12-30, 23:13:54
date modified: 2022-12-30, 23:16:18
---

什么时候遇到注解继承的问题？

背景：在项目中，为了方便其他微服务进行 [[OpenFeign]] 调用，有一个 Feign 接口类，其中定义了该微服务 Controller 中的所有接口，且 Controller 实现了该 Feign 接口。

```java
@FeignClient
public interface User {
    @GetMapping
    UserInfo getUserInfo();
}

@RestController
public class UserController implements User {
    @Override
    public UserInfo getUserInfo() {
        // ...
    }
}
```

错误行为：在 Feign 接口中定义完新接口后，Controller 仅 Override 实现，没有标上例如 `@GetMapping` 等常规注解。结果实际调用时报 404 异常。

错误认知：以为子类继承了父类方法上的注解。

正确行为：像以前一样常规定义 Controller 中的接口，只是多加一个 `@Override` 注解而已。
