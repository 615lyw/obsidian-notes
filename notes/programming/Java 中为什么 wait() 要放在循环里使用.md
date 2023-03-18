---
date created: 2021-12-30, 10:30:58
date modified: 2022-05-30, 17:19:35
---

# Meta

- parent :: [[线程的基本操作#wait]]，[[管程模型]]
- siblings ::
- child ::
- refs:
    - [08 | 管程：并发编程的万能钥匙-极客时间](https://time.geekbang.org/column/article/86089)
    - JDK Object wait 文档

---

原因：

- Java 实现的是 MESA 风格管程
- 存在 spurious wakeup（虚假唤醒）的情况，即没有其他线程执行 `notify`，挂在条件变量上的线程却被唤醒

```java
synchronized(obj) {
    while (condition does not hold) {
        obj.wait();
    }
    // perform action appreopriate to condition
}
```
