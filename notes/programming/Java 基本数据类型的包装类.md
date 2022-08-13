---
date created: 2022-06-08, 15:49:12
date modified: 2022-06-08, 15:50:42
---

# Meta

- parent :: [[Java 基础语法]]
- siblings ::
- child ::
- refs:

---

Byte，Short，Integer，Long，Character 的 `valueOf` 方法利用了 [[享元模式 Flyweight]] 缓存常用值。

Float 和 Double 则直接 new 的对象没有使用缓存。
