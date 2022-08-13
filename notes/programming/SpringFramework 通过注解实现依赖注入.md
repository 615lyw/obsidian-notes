# 依赖注入

`@Autowired` 位置不同：在属性、setter 方法、构造方法上。

## 属性注入

缺点：在非 IoC 框架下无法使用，除非通过反射赋值，否则会抛 NPE。

```java
@Controller
public class FooController {
  @Autowired
  private FooService fooService;
  
}
```

## setter 注入

好处：

- bean 初始化后能动态替换依赖
- 语义上表示是一个可选、可替换依赖
- 提高代码的可测试性（ [[如何提高代码的可测试性]] ）

```java
@Controller
public class FooController {
  
  private FooService fooService;
  
  @Autowired
  public void setFooService(FooService fooService) {
      this.fooService = fooService;
  }
}
```

## 构造器注入

```java
@Controller
public class FooController {
  
  private final FooService fooService;
  
  @Autowired
  public FooController(FooService fooService) {
      this.fooService = fooService;
  }
}
```

好处：

- 保证依赖不可变：通过 [[final 关键字]] 实现
- 保证依赖不为空+尽早警告存在 [[循环依赖问题]] ：当 bean 只有有参构造器时，Spring 在依赖注入时会检查所需依赖是否存在，若无则抛 `BeanCurrentlyInCreationException：Requested bean is currently in creation: Is there an unresolvable circular reference?`（属性注入和 setter 注入不会在 Spring 启动时报错，只在用到时才抛 NPE，不符合 [[尽早发现错误以节约时间和成本]] ）
- 保证该 bean 在使用时一定是完全初始化状态：因为依赖不为空且所需依赖完全初始化

缺点：

- 如果依赖过多会导致构造方法参数太多，显得笨重（可能是类不符合 [[单一职责原则 SRP]]，可通过 [[重构]] 解决）
