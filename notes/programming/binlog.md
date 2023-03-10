---
date created: 2022-06-20, 11:48:39
date modified: 2022-07-27, 11:16:27
---

# Meta

- alias:
- parent :: [[MOC MySQL]]
- siblings :: [[redo log]]，[[undo log]]
- child ::
- refs: [MySQL 日志：undo log、redo log、binlog 有什么用？ | 小林coding](https://xiaolincoding.com/mysql/log/how_update.html#%E4%B8%BA%E4%BB%80%E4%B9%88%E9%9C%80%E8%A6%81-binlog)

---

binlog 是 binary log 的缩写，即二进制日志。特点：

- Server 层日志，所有存储引擎都可以使用
- 记录了所有数据库表结构变更和表数据修改的日志，**不会记录查询类的操作**
- binlog 是追加写，写满一个文件，就创建一个新的文件继续写，不会覆盖以前的日志，保存的是全量的日志

# 用途

- [[MySQL 主从复制]]
- 备份
- 恢复

# binlog 什么时候刷盘？

事务执行过程中产生的 binlog 写入 Server 层的 binlog cache 中，事务提交时再从 binlog cache 中写入 binlog 文件中。

https://dev.mysql.com/doc/refman/5.7/en/replication-options-binary-log.html#sysvar_sync_binlog

`sync_binlog=1` default, Enables synchronization of the binary log to disk before transactions are committed.

# 两阶段提交

[[redo log 和 binlog 的两阶段提交]]