# 语言规范与实现

编程语言都有语言规范，我们写的每行代码都要符合语言规范，而语言的编译，解释，执行都与特定的实现有关。

> Java Specification Requests (JSRs) are the actual descriptions of proposed and final specifications for the Java platform.

JSR 提出具体的语言规范（描述语言的语法与语义），提交到 JCP (Java Community Process) 审核通过后才成为正式规范。

JSR 成为最终文件后（Final Release），会提供一份参考实现 RI (Reference Implementation) 以及技术兼容测试工具集 (TCK, Technology Compatibility Kit) 用于检测具体实现是否兼容。

故各厂商会根据 JSR 以及参考 RI 来实现自己的产品，最后通过 TCK 后便能使用 Java 商标了。[[商标概念]]

---

JVM 同样也有规范，HotSpot 是 JVM 规范的一种实现。
