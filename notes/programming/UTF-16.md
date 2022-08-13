---
date created: 2022-05-25, 16:44:14
date modified: 2022-05-31, 10:43:24
---

# Meta

- parent :: [[UTF]]
- siblings:: [[UTF-8]]
- child::
- refs: [UTF-16 - 维基百科，自由的百科全书](https://zh.wikipedia.org/wiki/UTF-16)

---

# UTF-16

UTF-16 表示一个 Unicode 字符需要 1 个或者 2 个 16 位长的 Code Unit 来表示，因此这是一个变长编码表示。

## BMP 层字符编码方式

**BMP 层所有字符只用一个 16 bit 的 Code Unit 表示，且数值等于对应字符的 Code Point。**

## 辅助平面字符编码方式

辅助平面（从 U+10000 到 U+10FFFF 的 Code Point）编码方式：

1. 被编码为一对 16 bit 的 Code Unit，称为 Surrogate Pair
2. …
