---
date created: 2022-12-28, 21:50:06
date modified: 2023-01-03, 00:05:41
---

# Meta

- alias: Netflix Eureka
- parent ::
- siblings ::
- child ::
- refs:

---

服务治理中心主要对各个服务实例进行管理，包括服务注册和服务发现。

概念：

- 微服务：完成某业务功能的系统
- 服务实例：一个微服务可以有多个服务实例，每个实例即具体的节点

# 微服务实例与 Eureka

在 Eureka 中主要由 Eureka 客户端主动维持和 Eureka 服务端的联系。

- 注册：
    - `spring.appliacation.name` 即微服务名
    - 微服务实例判断配置 `eureka.client.register-with-eureka` 决定是否通过 REST 请求注册到 Eureka 服务端
    - 注册地址根据 `eureka.client.serviceUrl.defaultZone` 生成
- 续约：
    - Eureka 客户端向 Eureka 服务端发送心跳，告知可用性，避免从服务列表中剔除
- 下线：
    - 服务正常停止时（IDEA Stop 服务），Eureka 客户端会 Eureka 服务端发送下线 REST 请求，取消注册

# Eureka
