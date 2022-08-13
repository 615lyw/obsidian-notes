---
date created: 2022-06-08, 15:33:03
date modified: 2022-06-08, 22:41:24
---

# Meta

- parent :: [[MOC 锁]]
- siblings :: [[String 不建议作为锁]]
- child ::
- refs: [28 | Immutability模式：如何利用不变性解决并发问题？-极客时间](https://time.geekbang.org/column/article/92856)

---

# Integer 等包装类不建议作为锁的原因

Integer 等包装类（[[Java 基本数据类型的包装类]]）利用了 [[享元模式 Flyweight]] 缓存了常用值，作为锁时可能出现重用情况。

```java
// 实际上，al 和 bl 共用了同一个对象，故 A 和 B 共用了一把锁。
class A {
  Long al = Long.valueOf(1);
  public void setAX(){
    synchronized (al) {
      //省略代码无数
    }
  }
}


class B {
  Long bl = Long.valueOf(1);
  public void setBY(){
    synchronized (bl) {
      //省略代码无数
    }
  }
}
```
