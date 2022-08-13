---
date created: 2022-01-23, 16:59:38
date modified: 2022-02-24, 14:03:09
---
# Meta

- alias:
- parent :: [[SpringBoot 自动装配原理]]
- siblings ::
- child ::
- refs: [Java常用机制 - SPI机制详解 | Java 全栈知识体系](https://pdai.tech/md/java/advanced/java-advanced-spi.html)

---

# 主要思想（功能）是什么？

为某个接口寻找实现。主要思想是将装配的控制权移到程序之外。好处是可以很好的实现模块化（SpringBoot 引入一个依赖/组件，实现自动装配）。

# 基本原理

1. 服务提供者实现接口
2. 在 classpath 下的 `META-INF/services/` 目录里创建一个以实现接口的全限定名命名的文件，文件内容为接口实现类的全限定名
3. 需要使用接口实现时，调用 `ServiceLoader.load(接口类.class)` 会根据 jar 包下的 `META-INF/services/` 中的配置文件中的实现类名进行加载（类加载机制）

# 具体例子

JDBC 4.0 后无需通过 `Class.forName("com.mysql.jdbc.Driver")` 手动加载驱动，而是 `Connection conn = DriverManager.getConnection(url, username, password);` 获取连接。

`DriverManager` 代码：
