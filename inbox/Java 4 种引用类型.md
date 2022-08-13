---
date created: 2022-08-03, 06:32:05
date modified: 2022-08-03, 06:36:32
---

# Meta

- alias:
- parent :: [[MOC JVM]]
- siblings ::
- child ::
- refs:
    - [Java垃圾回收 - czwbig - 博客园](https://www.cnblogs.com/czwbig/p/11127159.html)
    - 《深入理解 Java 虚拟机》

---

# 强、软、弱、虚引用

（以下内容直接摘抄自 refs）

JDK1.2 以前，一个对象只有被引用和没有被引用两种状态。

后来，Java 对引用的概念进行了扩充，将引用分为强引用（Strong Reference）、软引用（Soft Reference）、弱引用（Weak Reference）、虚引用（Phantom Reference）4 种，这 4 种引用强度依次逐渐减弱。

- 强引用就是指在程序代码之中普遍存在的，类似 `Object obj=new Object()` 这类的引用，垃圾收集器永远不会回收存活的强引用对象。
- 软引用：还有用但并非必需的对象。在系统 将要发生内存溢出异常之前 ，将会把这些对象列进回收范围之中进行第二次回收。
- 弱引用也是用来描述非必需对象的，被弱引用关联的对象 只能生存到下一次垃圾收集发生之前 。当垃圾收集器工作时，无论内存是否足够，都会回收掉只被弱引用关联的对象。
- 虚引用是最弱的一种引用关系。 无法通过虚引用来取得一个对象实例 。为一个对象设置虚引用关联的唯一目的就是能在这个对象被收集器回收时收到一个系统通知。

![tpwHrI](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/tpwHrI.png)
