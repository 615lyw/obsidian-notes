---
date created: 2022-01-10, 20:24:43
date modified: 2022-05-31, 16:32:03
---

# Meta

- parent :: [[MOC 锁]]
- siblings :: [[线程的基本操作]]
- child ::
- refs:

---

# synchronized

- synchronized + wait + notify 或 notifyAll 是 Java 对 [[管程模型]] 的一种实现
- synchronized 是 [[公平锁与非公平锁|非公平锁]]
- synchronized 是 [[可重入锁]]

## 用法

两种使用方式：

- 同步方法：修饰静态方法、实例方法
- 同步代码块

写法：

```java
public static synchronized void foo() {
	// 锁对象为 Class
}

public synchronized void bar() {
	// 锁对象为 this
}

public void test() {
	// 锁对象为任意对象
	synchronized(obj) {
		
	}
}
```

Java 编译器会在 synchronized 修饰的方法或代码块前后自动加上加锁和解锁，保证加锁解锁操作成对出现。

加/解锁中的锁对象：

- 静态方法为：Class 对象
- 实例方法为：this 对象

## 作用

解决了原子性和可见行问题。

原子性：临界区的代码一定会保证逻辑上一次性执行完毕。

可见行：

1. 根据 [[内存模型-JMM#Happens-Before 规则#程序顺序性规则]] 对共享变量的修改对于解锁操作是可见的
2. 根据 [[内存模型-JMM#Happens-Before 规则#管程锁规则]] 解锁对于加锁是可见的
3. 线程 A 对共享变量的修改 > 解锁 > 加锁 > 线程 B 对共享变量的修改

## 线程被唤醒后从哪开始执行

```java
synchronized (obj) {
    code1;
    if (条件不满足)
      obj.wait();
    code2;
}
```

当调用 `wait()` 时线程阻塞，被唤醒时，直接执行 code2。

线程被唤醒后进行入口等待队列只是为了竞争锁，竞争到锁后并非从头开始执行临界区代码，而是从 `wait` 下一行开始执行。

## synchronized 是简单版 [[管程模型]] 实现

锁对象与条件变量是同一个对象。

## 底层实现

“底层是在临界区代码前后加上 lock 和 unlock 操作，底层实现为在锁对象的对象头 MarkWord 部分写入获得锁对象的线程 id。”

当线程要进入 synchronized 代码块时要先获得指定的对象锁，直到该线程执行完 synchronized 区时才会释放该对象锁。

假设线程 A 在执行 synchronized 代码块时时间片用完，OS 切换为线程 B，线程 B 也需要执行 synchronized 区，故需先获得锁对象，但由于线程 A 没有释放锁对象，故线程 B 阻塞 [[Java 线程生命周期#BLOCKED]]。
