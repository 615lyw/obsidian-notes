# String、StringBuilder、StringBuffer

String **不可变性**：`private final char value[]`

StringBuilder 和 StringBuffer 继承自 AbstractStringBuilder，而 AbstractStringBuilder 内部字符串的保存没有使用 `final` 修饰，故 StringBuilder 和 StringBuffer 可变：

```java
/**
* The value is used for character storage.
*/
char[] value;
```

因为 String 对象内容是不可变的，故线程安全。

StringBuffer 的方法都加了同步锁，故线程安全。

StringBuilder 的方法没有加同步锁，故线程不安全，但性能要略高。

AbstractStringBuilder 中定义了一些字符串基本操作，如 append、insert 等，故当涉及大量的字符串修改操作时，StringBuilder 和 StringBuffer 要优于 String，因为不用频繁创建新的 String 对象。
