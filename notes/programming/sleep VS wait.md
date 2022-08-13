# sleep 和 wait 的异同点

相同点：两者都可让线程暂停。

不同点：在 synchronized 代码中调用 `sleep()` 不会释放锁，而 `wait()` 会释放锁。
