---
date created: 2022-07-18, 08:00:50
date modified: 2022-07-18, 08:01:33
---

# Meta

- alias:
- parent :: [[内存模型-JMM]]
- siblings ::
- child ::
- refs: [02 | Java内存模型：看Java如何解决可见性和有序性问题-极客时间](https://time.geekbang.org/column/article/84017)

---

# Happens-Before 规则

## 如何理解 A Happens-Before B？

A 操作的结果对于 B 是可见的。

## 程序顺序性规则

在一个线程内，按照程序顺序，前面的操作 Happens-Before 后面的操作。即线程内，前面对一个变量进行写操作，对于后面的读操作是可见的。

## 传递性规则

A Happens-Before B，B Happens-Before C，则 A Happens-Before C。

## volatile 变量规则

对一个 [[volatile|volatile]] 变量的写操作，Happens-Before 于后续对此变量的读操作。

在线程 A 对 volatile 变量进行写操作，在线程 B 中对此变量进行读操作一定可以获取最新值。

结合程序顺序性规则可知，volatile 变量写之前的任何操作也 Happens-Before 后续对此 volatile 变量的读操作。

```java
if (variable_a) {
    // 对 volatile 写
    volatileVariable = true;
}
```

## [[管程模型|管程]] 中锁的规则

一个锁的解锁 Happens-Before 后续对这个锁的加锁。注意前后锁需要为同一把锁。

## 线程 start 规则

主线程 A 启动子线程 B 后（执行子线程的 start 方法），子线程 B （run 方法里的所有操作）能够看到主线程在启动子线程 B 前的操作。

即主线程中的 start 操作 Happens-Before 子线程中的任意操作。

```java
Thread B = new Thread(()->{
  // 主线程调用B.start()之前
  // 所有对共享变量的修改，此处皆可见
  // 此例中，var==77
});
// 此处对共享变量var修改
var = 77;
// 主线程启动子线程
B.start();
```

## [[线程的基本操作#join 等待线程执行完毕|线程 join]] 规则

如果在线程 A 中调用线程 B 的 join 并成功返回，则线程 B 中的任何操作（run 方法里的所有操作）都 Happens-Before 此 join 操作。

## Happens-Before 规则应用范式

Happens-Before 规则在使用时基本由 3 条规则组合：

1. 程序顺序性规则
2. 除顺序性和传递性之外的任一规则
3. 传递性规则

3 条规则结合可以计算任意可见性问题。
