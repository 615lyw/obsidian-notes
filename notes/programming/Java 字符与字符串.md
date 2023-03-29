---
date created: 2022-03-31, 14:03:48
date modified: 2023-03-29, 20:50:48
---

# Meta

- parent:: [[Java 基础语法]]
- siblings::
- child::
- refs:
    - [字符和字符串 - 廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/1252599548343744/1255938912141568)
    - 《Java 核心技术卷 I - 第三章 - 数据类型》

---

# Java 字符与字符串

`char` 为基本数据类型，大小为 2 字节，[[Class Character]] 为其包装类。

> **更新（2022-05-25）**：char 保存的是字符的 [[UTF-16]] 编码！
>
> 之前以为是 Unicode Code Point，后参考 《Java 核心技术卷 I- 第三章 - 数据类型 -Unicode 与 char》以及 UFT-16 编码方式，发现在 BMP 层字符的 Unicode Code Point 恰好等于 UTF-16 编码后的 Code Unit。

字符的 Unicode Code Point 与字符的二进制编码序列中间还存在一层 Unicode 编码实现方案（[[Unicode#实现方式]]）。

一个英文字符或一个中文字符都可以用一个 `char` 表示。

JDK 9 之前，`String` 底层实现为 `char[]`，由上可知 `char` 对应一个字符，`length()` 方法返回的是 `char[]` 的长度，即字符个数（除了表情符号，绝大多数常用文字都在 Unicode BMP 层，两个字节足以表示）。

但当计算包含表情符号的字符长度时，`length()` 方法返回值不正确，因为一个表情一般为 4 个字节的 Unicode 码点，需要两个 `char` 字符才能表示，故更准确的是使用 `codePointCount` 方法。

```java
String str = "abc-你好-👋";
System.out.println(str.length()); // 9
System.out.println(str.codePointCount(0, str.length())); // 8
```
