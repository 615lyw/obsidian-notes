---
date created: 2021-11-15, 12:03:12
date modified: 2022-05-25, 16:04:29
---

# Java IO

![BmSimj](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/BmSimj.jpg)

# 什么是输入流/输出流？

# 依据传输类型分类

根据流中传输的数据类型可分为：

- 字节流
- 字符流（字节流的一种特殊形式）

字节流抽象基类：

- InputStream
- OutputStream

字符流抽象基类：

- Reader
- Writer

输入与输出都是相对于 JVM 来说。
输入：流入 JVM
输出：流出 JVM

# 依据功能分类

# 字节流转换为字符流

字节流如何转换成字符流？

`Reader reader = new InputStreamReader(inputStream);`
