---
date created: 2022-05-18, 21:14:59
date modified: 2022-05-23, 14:30:30
---

# Meta

- parent:: [[反射]]
- siblings::
- child::
- refs:

---

# ClassLoader

> A class loader is an object that is responsible for loading classes. The class ClassLoader is an abstract class. Given the binary name of a class, a class loader should attempt to locate or generate data that constitutes a definition for the class. A typical strategy is to transform the name into a file name and then read a "class file" of that name from a file system.

ClassLoader 是 [[双亲委派模型]] 中负责加载 class 文件至 JVM 中的核心类。

ClassLoader 是一个抽象类，作用：

1. 能通过文件系统读取指定文件，故可以用来读取 class 文件或资源文件
2. 能将 class 文件转换为 JVM 可识别的 Class 对象

# 如何获取 ClassLoader？

在 Class 对象上（[[反射#获取 Class 对象的 3 种方式|获取 Class 对象的 3 种方式]]）调用 `public ClassLoader getClassLoader()` 获取 ClassLoader 实例。

# 线程与类加载器的关系

每个线程都有一个上下文类加载器。

**主线程的上下文类加载器是系统类加载器。**

创建新线程时，新线程的上下文类加载器会被设置为父线程的上下文类加载器。

# 类加载器是命名空间的一部分

唯一确定一个类：类加载器 + 全类名。
