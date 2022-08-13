---
date created: 2022-01-10, 20:24:00
date modified: 2022-06-12, 10:31:34
---

# Meta

- parent :: [[通用线程生命周期]]
- siblings ::
- child ::
- refs: [09 | Java线程（上）：Java线程的生命周期-极客时间](https://time.geekbang.org/column/article/86366)

---

# Java 线程生命周期

线程在 JVM 中的状态在 Thread 类中的 State 枚举中定义了。（[[Class Thread#线程状态]]）

Java 线程生命周期在 [[通用线程生命周期]] 的基础上进行了简化或细化：

![[Pasted image 20220225223524.png]]

## NEW（初始化状态）

通过 `new Thread()` 创建了一个新线程，但还未调用 `start()`。[[创建线程的 2 种方式]]

## RUNNABLE（可运行态 / 运行态）

调用线程的 start 方法即可转变为 RUNNABLE 状态。

---

blocked、waiting、timed_waiting 均对应通用生命周期中的阻塞状态，可以理解为线程阻塞的原因。

## RUNNABLE -> BLOCKED

线程执行 synchronized 代码没有竞争到锁对象而阻塞。

## RUNNABLE -> WAITING（无限时等待）

- `Object.wait()`
- `Thread.join()`
- `LockSupport.park()`

只能通过 `interrupt()`、`notify()` 和 `notifyAll()` 唤醒转换为 `RUNNABLE`。

## RUNNABLE -> TIMED_WAITING（有限时等待）

- `Thread.sleep(long millis)`
- `Object.wait(long timeout)`
- `Thread.join(long millis)`
- `LockSupport.parkNanos(Object blocker, long deadline)`
- `LockSupport.parkUntil(long deadline)`

相比 WAITING 状态，仅为触发条件多了超时参数。

到达指定时间后线程会转换为 `RUNNABLE`。

---

## TERMINATED（终止态）

线程 `run()` 执行结束或线程内抛出异常。
