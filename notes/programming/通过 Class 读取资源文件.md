---
date created: 2022-05-18, 21:14:59
date modified: 2022-05-23, 15:00:56
---

# Meta

- parent :: [[ClassLoader]]
- siblings :: [[如何读取资源文件]]
- child ::
- refs: Java 源码

---
通过 `Class.getResourceAsStream()` 读取资源底层实际上是委派给 `ClassLoader.getResourceAsStream()`。

Class 相比 ClassLoader 更方便使用，因为封装了对资源文件名的处理。（[[通过 ClassLoader 或 Class 读取资源文件的区别]]），故待读取资源文件名 resourceName 写法：

1. 如果资源文件就在 resources 直接目录下，在文件名前加 `/`，如 `/file.txt`
2. 如果文件在 resources 更深目录下，则以 `/` 开头加上每层目录，如： `/com/liuyiwei/config.xml`
3. 如果读取文件的代码所在包与待读取文件所在 resources 下包目录结构一样，才直接写文件名，如 `hello.txt`
