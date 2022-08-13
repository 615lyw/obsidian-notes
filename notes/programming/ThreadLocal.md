---
date created: 2022-08-02, 10:36:43
date modified: 2022-08-03, 06:14:21
---

# Meta

- alias:
- parent :: [[Java 并发编程]]
- siblings ::
- child ::
- refs: [30 | 线程本地存储模式：没有共享，就没有伤害-极客时间](https://time.geekbang.org/column/article/93745?cid=100023901)

---

# 思想

ThreadLocal 思想是避免线程间共享，顾名思义：创建专属于线程自己的局部变量。（局部变量是线程安全的）

# 作用

- 将某个变量与线程关联起来，在此线程的整个执行上下文中都可以访问到该变量，可以理解为线程的执行上下文
- 避免线程间共享，从而实现线程安全

# 原理

从语义上可以理解为：`ThreadLocal<T>` 中有一个 `Map<Thread, T>`，保存了特定于线程的值。

线程调用 ThreadLocal 实例的 `get()` 方法，源码如下：

```java
public T get() {
    Thread t = Thread.currentThread();
    ThreadLocalMap map = getMap(t); // 获取当前线程的 ThreadLocalMap 成员
    if (map != null) {
        // 线程的 ThreadLocalMap 成员以 ThreadLocal 实例作为 key!!!
        // value 是 ThreadLocal<T> 中 T 对应的元素
        ThreadLocalMap.Entry e = map.getEntry(this);
        if (e != null) {
            @SuppressWarnings("unchecked")
            T result = (T)e.value;
            return result;
        }
    }
    return setInitialValue(); // 此 get 方法有创建本地存储变量的功能
}


ThreadLocalMap getMap(Thread t) {
    return t.threadLocals;
}


private T setInitialValue() {
    T value = initialValue(); // ThreadLocal 实例会重写此方法
    Thread t = Thread.currentThread();
    ThreadLocalMap map = getMap(t);
    if (map != null) {
        map.set(this, value); // Key 只能是当前 ThreadLocal 实例
    } else {
        createMap(t, value);
    }
    if (this instanceof TerminatingThreadLocal) {
        TerminatingThreadLocal.register((TerminatingThreadLocal<?>) this);
    }
    return value;
}

public void set(T value) {
    Thread t = Thread.currentThread();
    ThreadLocalMap map = getMap(t);
    if (map != null) {
        map.set(this, value);
    } else {
        createMap(t, value);
    }
}
```

# ThreadLocal 的使用

1. 创建一个 ThreadLocal 对象
2. 重写其 `initialValue()` 方法或调用静态方法 `ThreadLocal.withInitial()` 为每个线程创建本地存储变量
3. 通过调用此 ThreadLocal 对象的 `get()` 方法获取每个线程内部存储的本地变量

# 以面向对象设计的视角来看 ThreadLocal 和 Thread 的关系

![CleanShot2022-08-02at14.08.11](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/CleanShot%202022-08-02%20at%2014.08.11.png)

- Thread 有一个 ThreadLocalMap 的成员
- 以每个 ThreadLocal 对象作为 key！一个线程可以有多个本地存储变量，需要建立多个 ThreadLocal 对象来操作

**ThreadLocal 仅为一个工具类，用于创建或获取线程的本地存储变量**，所有和线程自身有关的数据保存在 Thread 内部。

# 内存泄漏

虚拟机栈是线程私有的内存区域，只要线程没有终止，就能确保它们中引用的对象的存活。=> 线程若终止，相关引用对象不可达，被回收。

**1 从语义上可以理解为：`ThreadLocal<T>` 中有一个 `Map<Thread, T>`，保存了特定于线程的值。为什么不这样实现呢？**

原因：避免 [内存泄漏](notes/programming/内存泄漏.md)。当线程终止后，保存在 Thread 中的本地存储值不可达，被垃圾回收。但若保存在 ThreadLocal 中，由于 ThreadLocal 生命周期长于 Thread，当线程终止后，仍存在对线程对应的本地存储值的强引用，故无法被垃圾回收。

**2 在 [线程池](notes/programming/线程池.md) 中使用 ThreadLocal 可能导致 [内存泄漏](notes/programming/内存泄漏.md)。**

引用链：`Thread -> ThreadLocalMap -> Entry[] -> Entry=(WeakReference ThreadLocal, Object value)`

线程池中的线程生命周期长于 ThreadLocal 的生命周期，当 ThreadLocal 到达自己的生命周期时（没有对 ThreadLocal 的强引用了，仅存在 [Java 4 种引用类型](inbox/Java%204%20种引用类型.md) 中的 WeakReference），ThreadLocal 可以被回收，Thread 中**被强引用的**本地存储变量由于 ThreadLocal 对象被回收而**不可达**造成内存泄漏。
