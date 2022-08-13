# 如何注入 Prototype 的 Bean 至单例的 Bean 中？

以往都是面向过程编程大部分代码集中在 Controller、Service、Dao 中，三个 bean 都是单例的，即单例 bean 之间互相注入。

如今面向对象编程，对象有自己的成员变量，为了防止线程安全问题，则对象不能是单例的，使用的时候需要 new 一个新对象，可以通过 `@Scope("prototype")` 注解实现。

```java
@RestController
public class FooController {
    @Autowired
    private FooService fooService;

    @RequestMapping("/foo")
    public void foo() {
        fooService.do();
    }
}

@Service
@Scope("prototype")
public class FooServiceImpl {
    
}
```

上面这种典型代码，虽然 FooServiceImpl 类上标了 `@Scope("prototype")` 注解，但在 Controller 中始终都是同一个 Service bean，由于单例的 Bean 在容器启动时就会完成一次性初始化 [[基于 XML 的 SpringFramework 使用#bean 的生命周期]]。

要实现标题所述有 2 种方法：

- `@Scope(value = "prototype", proxyMode = ScopedProxyMode.TARGET_CLASS)`
- 从 `ApplicationContext` 中手动获取 bean

## 通过给 @Scope 注解的代理模式解决

代理模式原理参考：[[动态代理]]

proxyMode 支持两种有效值：

- `ScopedProxyMode.TARGET_CLASS`
    - 表示被代理类为一个普通类
    - 通过 [[CGLIB]] 实现
- `ScopedProxyMode.INTERFACES`
    - 表示被代理类是一个接口
    - 通过 [[JDK 代理]] 实现

大致原理：单例 bean 中注入的是代理对象，访问代理对象的任意方法时，Spring 会创建一个新对象，在新对象上调用被访问的方法。

**坑：因为在代理对象上调用任意方法都会返回一个新对象，但我们可能错误理解还是同一个对象导致出现异常。**

面向对象编程中，一个对象有自己的内部状态，故两个成员方法之间可能共享成员变量。这种情况下如果在代理对象上分别调用两个共享成员变量的方法会导致异常。

## 通过 ApplicationContext 手动获取

通过 `@Autowired` 注入一个 ApplicationContext，再通过 `applicationContext.getBean(Class clz)` 获取想要的 bean，这样每次获取到 bean 都是新创建的。

# 参考

https://www.jianshu.com/p/54b0711a8ec8
