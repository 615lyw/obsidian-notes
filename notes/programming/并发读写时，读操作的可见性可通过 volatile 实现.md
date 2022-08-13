---
date created: 2022-06-11, 09:36:09
date modified: 2022-06-11, 09:36:19
---

# Meta

- alias:
- parent :: [[并发读写时，读操作也需要加锁]]
- siblings ::
- child ::
- refs: [关键字: volatile详解 | Java 全栈知识体系](https://pdai.tech/md/java/thread/java-thread-x-key-volatile.html#%E6%A8%A1%E5%BC%8F5%EF%BC%9A%E5%BC%80%E9%94%80%E8%BE%83%E4%BD%8E%E7%9A%84%E8%AF%BB%EF%BC%8D%E5%86%99%E9%94%81%E7%AD%96%E7%95%A5)

---

```java
public class CheesyCounter {
    private volatile int value;
 
    public int getValue() { return value; }
 
    public synchronized int increment() {
        return value++;
    }
}
```

相比给读操作加锁来保证可见性，volatile 实现可见性开销更小。