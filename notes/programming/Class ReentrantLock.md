---
date created: 2021-11-29, 14:27:57
date modified: 2022-06-11, 19:19:44
---

# Meta

- parent :: [[MOC 锁]]，[[Interface Lock]]
- siblings ::
- child ::
- refs:
---

# Class ReentrantLock

- `java.util.concurrent` 包中的锁
- ReentrantLock 是 [[可重入锁]]
- 默认构造是 [[公平锁与非公平锁|非公平锁]]，但也可以设置为公平锁
- 一个 ReentrantLock 可以同时绑定多个 [[Interface Condition|Condition]] 对象
- ReentrantLock 用于解决互斥问题，Condition 用于解决线程间同步问题
- 基于 [[AQS]] 实现
