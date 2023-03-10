---
date created: 2023-03-10, 10:26:10
date modified: 2023-03-10, 10:34:40
---

# Meta

- alias:
- parent :: [[redo log]], [[binlog]]
- siblings ::
- child ::
- refs: [xiaolingcoding-为什么需要两阶段提交？](https://xiaolincoding.com/mysql/log/how_update.html#%E4%B8%BA%E4%BB%80%E4%B9%88%E9%9C%80%E8%A6%81%E4%B8%A4%E9%98%B6%E6%AE%B5%E6%8F%90%E4%BA%A4)

---

为什么需要两阶段提交？

保证 redo log 和 binlog 逻辑一致。

![[Pasted image 20230310103639.png]]

1. 先写 redo log，并标记为 prepare
2. 写 binlog 成功后，标记 redo log 为 commit