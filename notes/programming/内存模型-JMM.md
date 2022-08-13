---
date created: 2022-01-25, 19:03:18
date modified: 2022-07-18, 08:00:59
---

# Meta

- parent :: [[Java 并发编程]]
- siblings:: [[volatile]]，[[final 关键字]]
- child::
- refs: [02 | Java内存模型：看Java如何解决可见性和有序性问题-极客时间](https://time.geekbang.org/column/article/84017)
---

# 内存模型 JMM

- Java 内存模型由 [JSR-133](https://jcp.org/en/jsr/detail?id=133) 定义（直到 JDK 5 才完善）
- 内存模型：在特定的缓存一致性协议下（比如 [[MESI 缓存一致性协议]]），对真实的内存或高速缓存进行读写访问的过程抽象
    - ![硬件内存模型](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/CleanShot%202022-06-17%20at%2011.01.42@2x.png)
    - ![JVM 内存模型](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/CleanShot%202022-06-17%20at%2011.02.48@2x.png)
    - 线程对共享值的所有操作（读取、赋值等）都必须在工作内存中进行，而不能直接读写主内存中的数据。 不同的线程之间也无法直接访问对方工作内存中的变量，线程间变量值的传递均需要通过主内存来完成（不准确，比如 CPU 之间也能直接交互，参考 [[MESI 缓存一致性协议]]）
- **不同 CPU 架构有不同的内存模型，JVM 通过定义更抽象的内存模型，屏蔽具体硬件和操作系统内存访问差别，实现跨平台**
- JMM 是 JVM 规范（[[Java 虚拟机规范]]）之一，通过定义多项规则按需禁用缓存以及约束编译器的指令重排优化行为（允许编译器优化，但优化后一定要符合 [[Happens-Before]] 规则）。解决 [[并发编程 bug 源头：可见性、原子性、有序性问题|可见性和有序性问题]]
- JMM 规范分为两部分：
    - 面向编写并发程序的应用开发人员，核心是 [Happens-Before](notes/programming/Happens-Before.md) 规则
    - 面向 JVM 实现人员
