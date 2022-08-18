---
date created: 2021-11-25, 17:14:46
date modified: 2022-08-18, 11:46:20
---

# Meta

- parent :: [[设计模式]]
- siblings ::
- child ::
- refs:
---

# 单例模式

单例实现主要分为：

- 饿汉式
- 懒汉式

# 1- 饿汉式

特点：

- 随类加载而创建实例
- 构造器私有化使得只有在该类中才能使用 new 操作符来创建实例

```java
public class Singleton1 {
    private static Singleton1 singleton1 = new Singleton1();
    
    private Singleton1() {}
    
    public static Singleton1 getInstance() {
        return singleton1;
    }
}
```

# 2- 懒汉式

线程不安全

```java
public class Singleton2 {
    private static Singleton2 singleton2;

    private Singleton2() {}

    private static Singleton2 getInstance() {
        if (singleton2 == null) {
            // 线程 1 刚执行完 if 判断就发生了线程切换, 线程 2 执行完剩下的语句拿到[1234]地址的singleton2对象
            // 线程 1 执行后续语句, 拿到[2345]地址的singleton2对象
            // 此时出现了两个singleton2对象
            singleton2 = new Singleton2();
        }
        return singleton2;
    }
}
```

# 3- 懒汉式

线程安全但性能不好

```java
public class Singleton {
    private static Singleton instance;
    
    private Singleton() {}
    
    // 解决了第一次创建时的竞争情况，后续使用其实不再需要 synchronized
    private static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

# 4- 懒汉式之 Double Check Lock（DCL）

```java
public class Singleton {
    private static volatile Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                // double check
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

## 指令重排序问题

以上写法存在指令重排序导致 [[并发编程 bug 源头：可见性、原子性、有序性问题|有序性问题]]，可通过 [[volatile]] 解决。

问题出在 `new` 操作符上：

预期中 `new` 的执行顺序：

1. 堆上分配一块内存 M
2. 在 M 上初始化 Singleton 对象
3. 把 M 的地址赋值给 instance 变量（volatile 写操作不能被前排，参考 [[volatile#作用（语义）]]）

指令重排序后：

1. 堆上分配一块内存 M
2. 把 M 的地址赋值给 instance 变量
3. 在 M 上初始化 Singleton 对象

假设执行到 2 时发生线程切换，另一个线程执行 `getInstance` 发现 `instance != null`，直接使用 instance 导致 NPE。

# 5- 懒汉式之静态内部类

静态内部类不会随着外部类的创建而创建

```java
public class Singleton {
    private Singleton() {}

    private static class InnerClass {
        static Singleton instance = new Singleton();
    }

    public static Singleton getInstance() {
        // JVM 来保证类加载过程是线程安全的
        return InnerClass.instance;
    }
}
```

# 6- 懒汉式之枚举

最完美的方式

```java
public enum Singleton {
    // 唯一的实例对象
    INSTANCE
}
```
