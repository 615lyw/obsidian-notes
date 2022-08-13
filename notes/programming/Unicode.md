---
date created: 2022-03-24, 16:59:21
date modified: 2022-05-25, 21:50:03
---

# Meta

- parent:: [[字符编码]]
- siblings::
- child::
- refs:
    - [Unicode - 维基百科，自由的百科全书](https://zh.wikipedia.org/wiki/Unicode)
    - [What's the difference between a character, a code point, a glyph and a grapheme? - Stack Overflow](https://stackoverflow.com/questions/27331819/whats-the-difference-between-a-character-a-code-point-a-glyph-and-a-grapheme#:~:text=A%20code%20point%20is%20the,of%20an%20encoded%20code%20point.)
    - [Unicode 查询网址](https://www.compart.com/en/unicode/)
    - [UTF-16 - 维基百科，自由的百科全书](https://zh.wikipedia.org/wiki/UTF-16)

---

# Unicode

Unicode 只是一个字符集，给每个字符定义了一个唯一整数，即码点（Code Point），但没有规定如何对字符进行编码。

字符 Unicode 书面表示法：`U+` 紧接一组十六进制的数字，BMP 层只需 4 个数字，其他层需要更多数字。

例如：

- `U+0024`
- `U+10437`

Unicode 共有 17 层，BMP 层范围为：0～65535，即 2^16，**BMP 层的所有字符可以通过 2 个字节表示。** BMP 层不是所有码点都用来映射字符，从 U+D800 到 U+DFFF 之间的码点没有映射的字符。

## Java 中如何表示一个 Unicode 字符？

BMP 层字符 `\uxxxx` 其中 xxxx 为 4 位十六进制数表示字符的 Code Point（等价于 Code Unit）。

BMP 层之外的辅助层，需要用 `\uxxxx\uxxxx` 两组 UTF-16 Code Unit 表示。

例如：`U+10437` => `\uD801\uDC37`。

```java
// 注意是十六进制:
char c3 = '\u0041'; // 'A'，因为十六进制0041 = 十进制65
Char c4 = '\u4e2d'; // '中'，因为十六进制 4e2d = 十进制 20013
```

## Code Point VS Code Unit

Code Point 即字符在 Unicode 中的唯一标识。

Code Unit 是字符编码的基本单元。UTF-8 的 Code Unit 为 8 bit，UTF-16 的 Code Unit 为 16 bit。例如一个中文在 UTF-8 中占 3 个字节，每个字节都是一个 Code Unit，3 个 Code Unit 表示一个中文 Code Point。而一个英文字符在 UTF-8 中占一个字节，此单一 Code Unit 表示一个英文 Code Point。

> - A code point is the atomic unit of information. Text is a sequence of code points. Each code point is a number which is given meaning by the Unicode standard.
> - A code unit is the unit of storage of a part of an encoded code point. In UTF-8 this means 8 bits, in UTF-16 this means 16 bits. A single code unit may represent a full code point, or part of a code point. For example, the snowman glyph (☃) is a single code point but 3 UTF-8 code units, and 1 UTF-16 code unit.

## 实现方式

- [[UTF-8]]
- [[UTF-16]]
