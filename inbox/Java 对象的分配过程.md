---
date created: 2022-08-14, 22:10:22
date modified: 2022-08-14, 22:10:26
---

# Meta

- alias:
- parent :: [MOC JVM](notes/programming/MOC%20JVM.md)
- siblings ::
- child ::
- refs: 《深入理解 Java 虚拟机》

---

1. 若对象所属的类还未加载，则先进行类加载
2. 在堆上分配内存空间
3. 给对象赋”零值”
4. 设置对象头
5. 执行构造方法（所属 Class 文件中的 `<init>()` 方法）