---
date created: 2022-06-07, 08:39:57
date modified: 2022-08-10, 13:25:52
---

# Meta

- parent ::
- siblings ::
- child ::
- refs:
    - 《UNIX 网络编程卷 I》
    - [IO 模型详解 | JavaGuide](https://javaguide.cn/java/basis/io.html#%E5%89%8D%E8%A8%80)
    - [Linux IO模式及 select、poll、epoll详解 - SegmentFault 思否](https://segmentfault.com/a/1190000003063859)

---

I/O 分类：

- 网络 I/O
- 文件 I/O

I/O 两阶段：

1. 操作系统等待数据到来或准备好
2. 将数据从内核空间复制到用户空间

# 阻塞 I/O

用户进程发起系统调用 `recvfrom`，上述两阶段均阻塞等待。

![[Pasted image 20230318192148.png]]

# 非阻塞 I/O

用户进程发起系统调用 `recvfrom`，若此时数据还未准备好会立即返回一个 error code，而不会阻塞用户进程。用户进程需要一直轮询检查数据是否准备好。当数据准备好时，用户线程还需等待数据从内核空间复制到用户空间。

> `recvfrom` 接受一个 flag 参数用于配置当没有可用数据时阻塞还是立即返回一个 error code。

![[Pasted image 20230318192127.png]]

# I/O 多路复用

select、epoll、poll 三者都是 I/O 多路复用系统调用。[[IO 多路复用-select、epoll、poll]]

用户进程调用 select 时，select 会去检查多个 socket，若存在某个 socket 数据已准备好，则返回；若没有则阻塞。

当有数据可用时，用户进程再调用 `recvfrom` 等待数据从内核空间拷贝到用户空间。

特点：一个用户进程能同时等待多个 socket，其中任意一个进入读就绪状态，则 select 函数返回。

![[Pasted image 20230318192533.png]]

# 异步 I/O

用户进程发起系统调用，操作系统立即返回，并不会阻塞用户进程。当数据已准备好且完成从内核空间复制到用户空间后，操作系统会给用户进程发起一个通知。

![[Pasted image 20230318195613.png]]

# 阻塞 I/O VS 非阻塞 I/O

在 I/O 两阶段中，数据从内核空间拷贝到用户空间时会阻塞用户进程的。

阻塞与非阻塞的区别在于，在数据准备阶段是否会阻塞用户进程。

# 同步 I/O VS 异步 I/O

POSIX 对两者的定义：

- 同步 I/O：用户进程在 I/O 操作（两阶段中任何一阶段）过程会导致进程阻塞
- 异步 I/O：用户进程在 I/O 操作过程中不会阻塞

# 总结

概念定义层级结构：

- 同步 I/O
    - 阻塞 I/O
    - 非阻塞 I/O
    - I/O 多路复用
- 异步 I/O

![CleanShot2022-06-07at08.38.58@2x](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/CleanShot%202022-06-07%20at%2008.38.58@2x.png)
