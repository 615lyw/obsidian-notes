---
date created: 2022-06-14, 11:56:58
date modified: 2022-06-27, 09:04:10
---

# Meta

- alias:
- parent :: [[InnoDB 存储引擎]]，[[事务]]
- siblings ::
- child ::
- refs:

---

- 原子性是通过 [[undo log]] 保证
- 一致性则是通过持久性 + 原子性 + 隔离性保证
- 隔离性：通过 [[MVCC]] 或 [[MySQL 锁|加锁]] 解决
    - 防止事务并发执行时可能会遇到的错误（脏写、脏读、不可重复读、幻读）
    - 脏写是通过加锁避免的
    - 脏读和不可重复读，分别通过 `READ COMMITTED` 和 `REPEATABLE READ` 隔离级别解决，这两个隔离级别通过 [[MVCC]] 实现
    - MySQL 通过 [[MySQL 锁#间隙锁]] 在 `REPEATABLE READ` 隔离级别下解决了幻读问题
- 持久性是通过 [[redo log]] 保证
