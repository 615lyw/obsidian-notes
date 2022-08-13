Meta:

- parents: [[Java 并发编程]]
- status: #Update
- refs: [22 | Executor与线程池：如何创建正确的线程池？-极客时间](https://time.geekbang.org/column/article/90771)
- create time: 2022-05-18 22:22:36
- update time:  2022-05-18 22:22:36

---

线程是重量级对象，为什么线程是重量级对象？

普通对象的创建仅为在 JVM 堆上分配一块内存，属于用户态操作；而创建线程需要调用操作系统 API，由用户态转为内核态，然后由操作系统为线程分配一系列资源，成本高，应避免频繁的创建与销毁，故引入线程池。
