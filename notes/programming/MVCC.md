---
date created: 2022-06-24, 18:15:13
date modified: 2022-08-10, 16:58:05
---

# Meta

- alias: Multi-Version Concurrency Control ，多版本并发控制
- parent :: [[MOC MySQL]]
- siblings ::
- child ::
- refs: [MySQL 是怎样运行的：从根儿上理解 MySQL - 小孩子4919 - 掘金课程](https://juejin.cn/book/6844733769996304392/section/6844733770071801870)

---

# 背景知识

使用 InnoDB 的表 B+ 树叶子节点中每条记录都有如下两个字段：

- trx_id：每次一个事务对某条聚簇索引记录进行改动时，都会把该事务的事务 id 赋值给该列
- roll_pointer：每次对某条聚簇索引记录进行改动时，都会把旧的版本写入到 undo 日志中，可以通过它来找到该记录修改前的信息

## 版本链

[[TODO]] 补图

# MVCC 原理

指的就是在使用 `READ COMMITTD`、`REPEATABLE READ` 这两种隔离级别的事务在执行普通的 `SELECT` 操作时遍历记录的版本链的过程。

当用户读取一行记录时，若该记录已经被其他事务占用，当前事务可以通过 undo 日志读取之前的行版本信息，实现非锁定读取。

## ReadView

1. 查询时会生成 ReadView，不同隔离级别生成 ReadView 的时机不同
2. ReadView 中有个关键内容：生成 ReadView 时系统活跃事务 id 列表
3. 查询过程中读取某条记录时，遍历该记录的版本链，比对记录的 trx_id 和 ReadView 中的活跃事务 id 列表决定该版本对当前事务是否可见

使用 `READ COMMITTED` 隔离级别的事务在每次 `SELECT` 查询开始时都会生成一个独立的 `ReadView`。（中途若有事务提交，则可读到修改的数据，所以每次读都要重新生成一个 ReadView，正因如此也会面临不可重复读问题）

对于使用 `REPEATABLE READ` 隔离级别的事务来说，只会在第一次执行 `SELECT` 查询语句时生成一个 `ReadView`，之后的查询就不会重复生成了。（只在第一次生成，后续如果有事务提交，也看不到已提交的修改数据）
