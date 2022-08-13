---
date created: 2022-06-09, 11:17:54
date modified: 2022-06-09, 11:18:21
---

# Meta

- parent :: [[Interface Lock]]，[[Interface Condition]]
- siblings ::
- child ::
- refs:

---

# Lock 与 Condition 关系（为什么是 `lock.newCondition()`）

本质上 Lock 和 Condition 是管程模型的一种实现，同 synchronized。

要想阻塞在 Condition 条件变量上，必须先获得 Lock 锁。

获取锁 `lock()`、释放锁 `unlock()`、在条件变量上阻塞 `await()` 或被唤醒 `signal()` 或 `signalAll()` 等机制与 [[synchronized]] + `wait()` + `notify()` 或 `notifyAll()` 相同。
