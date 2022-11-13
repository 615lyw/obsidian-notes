---
date created: 2022-07-05, 21:00:31
date modified: 2022-07-05, 21:00:37
---

# Meta

- alias:
- parent :: 
- siblings ::
- child ::
- refs: 
    - [SpringBoot 自动装配原理详解 | JavaGuide](https://javaguide.cn/system-design/framework/spring/spring-boot-auto-assembly-principles.html)
    - [Java常用机制 - SPI机制详解 | Java 全栈知识体系](https://pdai.tech/md/java/advanced/java-advanced-spi.html)

---

自动装配：引入 starter，在 `application.yml` 中简单配置一下即可直接使用，避免繁琐的全量配置过程。

`@SpringBootApplication` 中核心注解 `@EnableAutoConfiguration`。

通过 SPI 机制搜索 jar 包下 `META-INF/spring.factories` 中的组件配置类的全类名。

这些类都是 `@Configuration` 注解的配置类，里面用 `@Bean` 注入按默认配置好的组件。（组件由别人配好了，通过引入依赖 + [[Java SPI 机制|SPI 机制]] 自动注入 Spring 容器）
