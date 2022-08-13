---
date created: 2022-06-08, 14:10:48
date modified: 2022-06-09, 10:56:44
---

# Meta

- parent :: [[线程安全实现方法]]
- siblings ::
- child ::
- refs: [28 | Immutability模式：如何利用不变性解决并发问题？-极客时间](https://time.geekbang.org/column/article/92856)

---

# 如何理解不变性？

- 对象一旦被创建之后，其状态就不再发生变化（无法通过引用改变对象状态）
- 变量一旦被赋值，就不允许修改了
    - 若为引用类型，不允许指向别的对象
    - 若为基本类型，不允许修改其值

# 例子

[[Class String]] 具备不变性。
