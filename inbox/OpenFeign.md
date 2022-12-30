---
date created: 2022-12-26, 09:50:20
date modified: 2022-12-26, 09:50:32
---

# Meta

- alias:
- parent ::
- siblings ::
- child ::
- refs: 
    - http://c.biancheng.net/springcloud/open-feign.html
    - https://www.baeldung.com/netflix-feign-vs-openfeign

---

# Feign 发展历史

Netflix Feign 是 Netflix 公司发布的一种实现负载均衡和服务调用的开源组件。Spring Cloud 将其与 Netflix 中的其他开源服务组件（例如 Eureka、Ribbon 以及 Hystrix 等）一起整合进 Spring Cloud Netflix 模块中，整合后全称为 Spring Cloud Netflix Feign。

> Feign 的依赖为 spring-cloud-starter-feign。

Feign 支持多种注解，例如 Feign 自带的注解以及 JAX-RS 注解等，但遗憾的是 Feign 本身并不支持 Spring MVC 注解。

2019 年 Netflix 公司宣布 Feign 组件正式进入停更维护状态，将其转移到开源社区成为 [OpenFeign/Feign](https://github.com/OpenFeign/feign)。

```java
interface GitHub {
  @RequestLine("GET /repos/{owner}/{repo}/contributors")
  List<Contributor> contributors(@Param("owner") String owner, @Param("repo") String repo);

  @RequestLine("POST /repos/{owner}/{repo}/issues")
  void createIssue(Issue issue, 
  @Param("owner") String owner, @Param("repo") String repo);
}
```

OpenFeign 是 Spring Cloud 对 Feign 的二次封装，它具有 Feign 的所有功能，并在 Feign 的基础上增加了对 Spring MVC 注解的支持，例如 @RequestMapping、@GetMapping 和 @PostMapping 等。

> OpenFeign 的依赖为 spring-cloud-starter-openfeign。

```java
@FeignClient("name")
public interface NameService {
    @RequestMapping("/")
    public String getName();
}
```

> 补充：Spring Cloud Finchley 及以上版本一般使用 OpenFeign 作为其服务调用组件。由于 OpenFeign 是在 2019 年 Feign 停更进入维护后推出的，因此大多数 2019 年及以后的新项目使用的都是 OpenFeign，而 2018 年以前的项目一般使用 Feign。

**综上：Spring Cloud OpenFeign 是对 Feign 的增强，增加了对 Spring MVC 注解的支持。（一般语境下没必要分得特别清楚）**

# 使用时注意事项

