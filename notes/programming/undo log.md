---
date created: 2022-06-20, 11:41:00
date modified: 2022-07-27, 10:28:13
---

# Meta

- alias: **撤销日志**，回滚日志
- parent :: [[MOC MySQL]]
- siblings :: [[redo log]]，[[binlog]]
- child :: [[redo log VS undo log]]
- refs:
    - [MySQL 是怎样运行的：从根儿上理解 MySQL - 小孩子4919 - 掘金课程](https://juejin.cn/book/6844733769996304392/section/6844733770067607566)
    - [MySQL 日志：undo log、redo log、binlog 有什么用？ | 小林coding](https://xiaolincoding.com/mysql/log/how_update.html#%E4%B8%BA%E4%BB%80%E4%B9%88%E9%9C%80%E8%A6%81-undo-log)

---

作用：

- 实现事务回滚，保证 [[事务]] 的原子性
- 实现 [[MVCC|MVCC]] 的关键因素之一

在事务还未提交的过程中，更新、删除、插入语句的执行都会生成对应的 undo 日志，当 MySQL 故障或手动 `rollback` 时会读取 undo log，**通过做反操作实现回滚**（类似 Git 的 revert 操作），故 undo log 本质为记录旧状态的**逻辑日志**。

不同的操作，需要记录的内容也是不同的，所以不同类型的操作（修改、删除、新增）产生的 undo log 的格式也是不同的。比如：

- 在插入一条记录时，把这条记录的主键值记下来，回滚时只需要把这个主键值对应的记录删掉
- 在删除一条记录时，把这条记录中的内容都记下来，回滚时再把由这些内容组成的记录插入到表中
- 在更新一条记录时，把被更新的列的旧值记下来，回滚时再把这些列更新为旧值








