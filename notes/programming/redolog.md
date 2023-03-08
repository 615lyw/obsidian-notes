---
date created: 2022-06-20, 11:11:33
date modified: 2022-07-28, 23:31:25
---

# Meta

- alias:
- parent :: [[MOC MySQL]]
- siblings :: [[undo log]]
- child ::
- refs:
    - [MySQL 日志：undo log、redo log、binlog 有什么用？ | 小林coding](https://xiaolincoding.com/mysql/log/how_update.html#%E4%B8%BA%E4%BB%80%E4%B9%88%E9%9C%80%E8%A6%81-redo-log)
    - [MySQL 是怎样运行的：从根儿上理解 MySQL - 掘金课程](https://juejin.cn/book/6844733769996304392/section/6844733770063626253)

---

# 作用

更新记录不是立马写入磁盘，而是更新在 [[Buffer Pool]] 中缓存的页面，标记为脏页，等到一个合适的时机由后台线程把脏页写回磁盘。因为 [[事务]] 的持久性，只要事务一提交对数据库的修改是永久的，但此时其实还没有真正刷盘，故需要一种机制来实现只要事务一提交，即使系统崩溃或断电也能保证系统恢复后事务提交的影响已生效。

在事务执行过程中，对数据库的每处修改记录成一条条顺序的 redo 日志，**在事务提交时顺序写入磁盘对应的 redo log 文件**。

事务一提交 redo log 就落盘了，而更新的数据此时还在内存中，即 WAL（Write-Ahead Logging）技术。

# redo log 的本质

物理日志，记录了事务提交后还未被持久化到磁盘的脏页数据。

# redo 日志写入过程

redo 日志是**物理日志**，记录了物理级别页面修改（页面的某偏移量位置修改了哪些字节）。

- Mini-Transaction 概念
    - 含义：对页面的一次原子操作，简称 `mtr`
    - 一个 `mtr` 可以包含一组原子性的 redo 日志或一条原子性的 redo 日志
        - 有的原子操作会生成一系列 redo 日志
        - 有的原子操作只生成一条 redo 日志
- 一个事务可以包含若干条语句，每一条语句其实是由若干个 `mtr` 组成，每一个 `mtr` 又可以包含若干条 redo 日志

`mtr` 执行时：

1. 修改 Buffer Pool 中的内存页数据
2. 生成一组原子性的 redo log

`mtr` 结束时把生成的 redo log 写入到 redo log buffer 中（并非在事务执行过程中将 redo log 刷盘）。

# 什么时候将 redo log buffer 中的 redo log 刷盘？

- 当 redo log buffer 中记录的写入量达到阈值时会触发刷盘（大于 redo log buffer 内存空间的一半）
- InnoDB 的后台线程每隔 1 秒，将 redo log buffer 持久化到磁盘
- 每次事务提交时将缓存在 redo log buffer 里的 redo log 直接持久化到磁盘（受参数 `innodb_flush_log_at_trx_commit` 控制，有多种策略可配置）

## innodb_flush_log_at_trx_commit 三种策略

- 参数 0：事务提交时，redo log 仍留在 redo log buffer 中，并不会写入 redo log 文件
- 参数 1（默认）：事务提交时，将 redo log buffer 中的 redo log 写入 redo log 文件并持久化到磁盘
- 参数 2：事务提交时，将 redo log buffer 中的 redo log 写入 redo log 文件（注意：写入文件并不代表持久化到磁盘上了，参考 [[Page Cache]]）

![CleanShot2022-07-27at10.43.41](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/CleanShot%202022-07-27%20at%2010.43.41.png)

## innodb_flush_log_at_trx_commit 为 0 和 2 的时候，什么时候才将 redo log 写入磁盘？

InnoDB 的后台线程每隔 1 秒：

- 针对参数 0 ：会把缓存在 redo log buffer 中的 redo log ，通过调用 `write()` 写到操作系统的 [[Page Cache]]，然后调用 `fsync()` 持久化到磁盘。所以参数为 0 的策略，MySQL 进程的崩溃会导致上一秒钟所有事务数据的丢失;
- 针对参数 2 ：调用 `fsync()`，将缓存在操作系统中 [[Page Cache]] 里的 redo log 持久化到磁盘。所以参数为 2 的策略，较取值为 0 情况下更安全，**因为 MySQL 进程的崩溃并不会丢失数据，只有在操作系统崩溃或者系统断电的情况下，上一秒钟所有事务数据才可能丢失。**

# redo log 写入文件的过程

写入 MySQL 数据目录下的 redo 日志文件组（redo log group）。

- 默认由 2 个 redo log file 组成，文件容量固定（可配置），**以循环写的方式写入**
- 当 Buffer Pool 的脏页刷新到了磁盘中，redo log 对应的记录没用了，可擦除
- write pos 表示 redo log 当前记录写到的位置，用 checkpoint 表示当前要擦除的位置
    - 当 write pos 追上 checkpoint 表示 redo log 文件满，会导致 MySQL 阻塞，无法执行更新记录操作
    - MySQL 需要执行一次 checkpoint 操作，把 Buffer Pool 中的脏页刷写入磁盘，移动 checkpoint 指针

![CleanShot2022-06-24at12.30.11](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/CleanShot%202022-06-24%20at%2012.30.11.png)

# 两阶段提交

