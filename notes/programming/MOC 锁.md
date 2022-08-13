---
date created: 2021-12-28, 09:42:08
date modified: 2022-06-11, 09:36:39
---

# Meta

- parent :: [[并发理论]]
- siblings ::
- child ::
- refs:
    - [03 | 互斥锁（上）：解决原子性问题-极客时间](https://time.geekbang.org/column/article/84344)
    - [04 | 互斥锁（下）：如何用一把锁保护多个资源？-极客时间](https://time.geekbang.org/column/article/84601)
---

# 锁

- [[锁通过互斥实现高级语言层面的原子性]]
- [[一把锁对应一个等待队列]]
- [[使用锁时要注意访问共享数据结构的所有读写方法]]
    - [[并发读写时，读操作也需要加锁]]
        - [[并发读写时，读操作的可见性可通过 volatile 实现]]
- [[使用锁的最佳实践：锁应该是私有的、不可变的、不可重用的]]
- 种类
    - [[Class ReentrantLock]] 与 [[synchronized]]
    - [[公平锁与非公平锁]]
    - [[可重入锁]]
- 风险
    - [[使用锁时要考虑性能问题]]
    - [[死锁]]
    - [[String 不建议作为锁]]
    - [[Integer 等包装类不建议作为锁]]
