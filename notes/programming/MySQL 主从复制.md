---
date created: 2022-07-27, 11:23:41
date modified: 2022-07-27, 11:32:35
---

# Meta

- alias:
- parent :: [[MOC MySQL]]
- siblings ::
- child ::
- refs: [MySQL 日志：undo log、redo log、binlog 有什么用？ | 小林coding](https://xiaolincoding.com/mysql/log/how_update.html#%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6%E6%98%AF%E6%80%8E%E4%B9%88%E5%AE%9E%E7%8E%B0)

---

实现了 [[高性能数据库集群-读写分离]]，写操作发给主库，读操作发给从库，主从之间通过 [[binlog]] 实现主从复制。

三个阶段：

- 主库写 binlog
- 主从间同步 binlog
- 从库回放 binlog

![CleanShot2022-07-27at11.32.22](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/CleanShot%202022-07-27%20at%2011.32.22.png)
