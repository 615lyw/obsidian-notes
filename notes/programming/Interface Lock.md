---
date created: 2022-05-31, 16:15:46
date modified: 2022-06-09, 11:18:31
---

# Meta

- parent :: [[Java 并发编程]]
- siblings :: [[Interface Condition]]
- child ::
- refs: [14 | Lock和Condition（上）：隐藏在并发包中的管程-极客时间](https://time.geekbang.org/column/article/87779)

---

Lock 用于解决线程间互斥问题。[[Interface Condition]] 用于解决线程间协作问题。

Lock 提供比 [[synchronized]] 更灵活的锁获取与释放。

```java
// 支持中断的API
void lockInterruptibly() throws InterruptedException;
// 支持超时的API
boolean tryLock(long time, TimeUnit unit) throws InterruptedException;
// 支持非阻塞获取锁的API
boolean tryLock();
```

# Lock 如何保证可见性？

内部持有一个 [[volatile|volatile]] 成员变量，通过 [[内存模型-JMM#volatile 变量规则]] 来保证可见性。

获取 Lock 锁时，读写 volatile 变量；释放 Lock 锁时，也读写 volatile 变量。

# 使用范式

```java
Lock lock = new ReentrantLock();
lock.lock();
try {
    // do something
} finally {
    lock.unlock();
}
```

# 实现原理

聚合一个同步器的子类实现互斥访问。[[AQS]]
