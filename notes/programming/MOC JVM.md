---
date created: 2022-01-25, 19:01:37
date modified: 2022-08-03, 06:36:38
---

# Meta

- alias:
- parent :: [[MOC Java]]
- siblings ::
- child ::
- refs:
    - [尚硅谷宋红康JVM全套教程（详解java虚拟机）_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1PJ411n7xZ)
    - 《深入理解 Java 虚拟机 - 第三版》

---

- [[JVM 运行时数据区域]]
- [[JVM 垃圾回收]]

# 类加载过程

# Java 线程与线程调度实现

> 以 HotSpot 为例，他的每一个 Java 线程都是直接映射到一个操作系统原生线程来实现的（1:1）。Java 使用的线程调度方式是抢占式调度。From 《深入理解 Java 虚拟机》P458

# 内存模型 JMM

JVM 虚拟机屏蔽底层平台的具体实现，给 Java 代码提供一个虚拟化的统一执行平台，得以实现 Java 跨平台特性。虚拟一台主机自然就要虚拟出内存模型，从虚拟机层面，而不是 OS 或硬件层面来思考代码的“底层”执行过程。

原子性变量操作：read、load、assign、use、store、write 以及 lock 和 unlock。

# 参考
