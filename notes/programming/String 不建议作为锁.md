---
date created: 2022-06-08, 15:33:03
date modified: 2022-06-08, 22:44:12
---

# Meta

- parent :: [[MOC 锁]]
- siblings :: [[Integer 等包装类不建议作为锁]]
- child ::
- refs: [28 | Immutability模式：如何利用不变性解决并发问题？-极客时间](https://time.geekbang.org/column/article/92856)

---

# String 不建议作为锁的原因

因为 [[Class String]] 具有不变性，故使用 `+`、`+=` 操作符或 `replace` 方法会生成新的对象，可能出现上锁后锁被替换导致其他线程也能进入临界区。
