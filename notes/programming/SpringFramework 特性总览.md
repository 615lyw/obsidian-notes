# Features Overview

了解 Spring 核心特性有两个目的：

- 知道 Spring 整合了哪些技术，需要的时候可以想到作为技术选型之一
- 纠正对 Spring 只是 IoC 容器的认知，Spring 像一个胶水框架，整合、简化、拓展了 Java 生态

核心特性：

- 数据存储
    - 对 JDBC 的整合
    - 事务抽象
    - O/R 映射
- Web 技术
    - Web Servlet 技术栈
    - Web Reactive 技术栈
- 技术整合
    - 远程调用：同步阻塞
    - Java 消息服务：异步非阻塞
    - Java 管理扩展
    - Java 邮件客户端
    - 本地任务
    - 本地调度
    - 缓存抽象
    - Spring 测试

# Spring 版本要求

![[Spring 对 Java SE 和 Java EE 的版本要求.png]]

# Spring 模块化设计

Spring 早期所有功能都是集成在一块的，后面开始把功能拆分成模块，实现按需引用。
