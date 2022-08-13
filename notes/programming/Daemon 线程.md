# 什么是主线程

主线程：执行 main 方法的线程。

# Daemon 线程

线程可分为：

- Daemon 线程
- 非 Daemon 线程

## Daemon 线程和前台线程的关系

**后台线程（Daemon 线程）都是为前台线程（非 Daemon 线程）服务的，若所有前台线程都执行完毕，后台线程没有存在必要，会随 JVM 自动终止。**

## 前台线程中创建的新线程是什么线程？

由前台线程产生的新线程也为前台线程；
由后台线程产生的新线程也为后台线程。

## 主线程是 Daemon 线程吗？

主线程是一个非 Daemon 线程，而 JVM 垃圾回收器、内存管理等都是 Daemon 线程。

## 如何把前台线程转换为 Daemon 线程？

可以通过 `setDaemon(true)` 把前台线程转换为后台线程。

## 例子

```
public class Test {
    public static void main(String[] args) {
        Thread thread = new Thread(() -> {
            while (true) {
                System.out.println("新线程运行ing...");
            }
        });
        // 若 setDaemon(true)，新线程便为 Daemon 线程
        // 主线程执行完 thread.start() 后便结束，由于所有前台线程执行完毕，后台线程便会随 JVM 自动终止
        // 现象为 System.out.println("新线程运行ing...") 执行一小段时间后程序便会终止
        thread.setDaemon(true);
        thread.start();
    }
}
```
