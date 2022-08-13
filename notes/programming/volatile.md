---
date created: 2022-06-26, 09:45:12
date modified: 2022-08-10, 15:20:56
---

# Meta

- parent :: [[Java 并发编程]]
- siblings ::
- child ::
- refs:
    - [02 | Java内存模型：看Java如何解决可见性和有序性问题-极客时间](https://time.geekbang.org/column/article/84017)
    - [轻量级的同步机制——volatile语义详解(可见性保证+禁止指令重排) - takumiCX - 博客园](https://www.cnblogs.com/takumicx/p/9302398.html)
    - [关键字: volatile详解 | Java 全栈知识体系](https://pdai.tech/md/java/thread/java-thread-x-key-volatile.html)
    - [8 volatitle - 深入浅出Java多线程](https://redspider.gitbook.io/concurrent/di-er-pian-yuan-li-pian/8)
    - 《深入理解 Java 虚拟机》-12.3.3-volatile
    - [[内存屏障]]

---

本文介绍了：

- volatile 的基本使用和语义
- 实现原理
    - 什么是内存屏障
    - 在什么时候插入内存屏障
    - 按什么规则插入内存屏障
    - 为什么按那个规则插入内存屏障

# volatile 关键字

JVM 提供的最轻量级同步方式。

## 修饰对象

- 成员变量
- 类静态变量

只有这两种变量才可能成为多线程访问的共享变量。

## 作用（语义）

- 禁止指令重排序
    - 维护 happens-before 关系（[[内存模型-JMM#volatile 变量规则]]），volatile 写入不能向前排，读取不能向后排
        - 对 volatile 变量的写入不能重排到写入之前的操作之前
        - 对 volatile 的读取操作不能被重排到后续操作之后
    - 经典问题：[[单例模式 Singleton#指令重排序问题]]
- 保证可见性
    - 写入 volatile 对于后续读 volatile 是可见的（volatile 变量规则）
    - 写入 volatile 之前的操作对于后续读 volatile 后的操作也是可见的（volatile 变量规则 + 程序顺序性规则）

注意，**volatile 不保证原子性！**

## 不保证原子性代码验证

```java
public class MyCounter {
    private volatile int count = 0;

    public void increment() {
        // 不保证原子性，可能出现写覆盖情况
        count++;
    }

    public int getCount() {
        // 读时一定是最新值
        return count;
    }
}

public class ConcurrencyTest {
    @Test
    public void testCounterWithConcurrency() throws InterruptedException {
        int taskNumber = 10000;
        ExecutorService service = Executors.newFixedThreadPool(10);
        CountDownLatch latch = new CountDownLatch(taskNumber);
        MyCounter counter = new MyCounter();
        for (int i = 0; i < taskNumber; i++) {
            service.execute(() -> {
                counter.increment();
                latch.countDown();
            });
        }
        latch.await();
        // Expected :10000
        // Actual   :9968
        assertEquals(taskNumber, counter.getCount());
    }
}
```

IDEA 还有提示：

![CleanShot2022-06-10at10.29.24@2x](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/CleanShot%202022-06-10%20at%2010.29.24@2x.png)

## 实现原理

通过 [[内存屏障]] 实现可见性和禁止指令重排序。

### 禁止指令重排序实现原理

**何时插入内存屏障？**

volatile 对生成字节码没有影响，即无论变量有没有 volatile 修饰，生成的字节码都是一样的，而是在根据字节码生成汇编代码（无论是解释执行还是 JIT 即时编译）时插入内存屏障。

**按什么规则插入内存屏障？**

- JVM 把字节码编译为汇编时，才会检查变量是否为 volatile 修饰的，如果是，则插入对应的 Barrier：
    - 在每个 volatile 写操作前插入一个 StoreStore 屏障
    - 在每个 volatile 写操作后插入一个 StoreLoad 屏障
    - 在每个 volatile 读操作后插入一个 LoadLoad 屏障
    - 在每个 volatile 读操作后再插入一个 LoadStore 屏障
- 最终生成汇编代码时，每个 Barrier 在不同平台实现不同，在 x86 平台，只有 StoreLoad 有效且通过 lock 指令实现，故最终表现为 volatile 写操作后有一个 lock 指令

**为什么要按照上述规则插入内存屏障？**

- 根据 volatile 读或写插入内存屏障规则表可知：
    - 根据第三行可知，volatile 读后需插入 LoadLoad 和 LoadStore 防止后面的操作重排至前面
    - 根据第四行和第四列可知，volatile 写后需插入 StoreLoad 和 StoreStore 以及 volatile 写之前需插入 LoadStore 和 StoreStore，因为 LoadStore 和 StoreStore 重复（第三行、第四行、第四列的交点），去重后可得 volatile 写之前插入 StoreStore，写之后插入 StoreLoad

![CleanShot2022-06-19at10.47.53@2x](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/CleanShot%202022-06-19%20at%2010.47.53@2x.png)

**上面这个规则表如何得到的？**

- 根据 volatile 写操作之前的任何操作 Happens-Before volatile 读操作（[[内存模型-JMM#volatile 变量规则]]），故写操作不能往前排，即在写操作前应插入内存屏障（对应第四列）
- 读 volatile 后根据读到的值做一些事情，那么读操作便不能往后排，故在后面插入内存屏障（对应第三行）
- 针对先 volatile 写，再 volatile 读的顺序情况，为了保证后面的读取能看到前面写入导致的变化，所以需要插入内存屏障（对应第四行第三列）

### 保证可见性实现原理

- 对于 volatile 变量本身（只是一个粗略模型，适用于简单 CPU 模型，如果是 [[MESI 缓存一致性协议]] 模型，则不准确）
    - 读取 volatile 变量不能使用缓存，每次读取都要去内存
    - 写入（缓存） volatile 变量后立即写回内存
- 对于 volatile 前后的操作，通过插入内存屏障禁止重排保证可见性
