---
date created: 2022-06-08, 08:56:58
date modified: 2022-06-17, 08:53:37
---

# Meta

- parent :: [[Java 字符与字符串]]
- siblings :: [[String、StringBuilder、StringBuffer]]
- child ::
- refs:
    - [Java基础常见知识&面试题总结(中)-String 为什么是不可变的 | JavaGuide](https://javaguide.cn/java/basis/java-basic-questions-02.html#string)
    - [28 | Immutability模式：如何利用不变性解决并发问题？-极客时间](https://time.geekbang.org/column/article/92856)

---

# Class String

```java
public final class String implements java.io.Serializable, Comparable<String>, CharSequence {
    private final char[] value;
	//...
}
```

## JDK 9 中为什么 String 底层存储由 `char[]` 改为 `byte[]`？

绝大部分字符串对象只包含 Latin-1 可表示的字符，Latin-1 编码方案下，byte 占一个字节 (8 位)，char 占用 2 个字节（16），byte 相较 char 节省一半的内存空间。

## String 具有不变性含义

- String 对象的内部属性一旦创建便不能再被修改，故为不可变对象（不可变类）
- 可变对象是可以通过方法修改其内部属性

使用 `+`、 `+=` 操作符或 `replace` 等修改方法会不会改变内部属性，而是生成新的对象。

## 如何实现不变性？

- 类被 [[final 关键字]] 修饰：禁止子类继承重写方法
- char 数组被 [[final 关键字]] 和 `private` 修饰：只在构造器中赋初值，后续不允许再次赋值（意味着引用变量一直没变）
- 暴露的公有方法均不会通过引用对 char 数组进行修改

replace 方法源码：

```java
public final class String {
  private final char value[];
  
  // 字符替换
  String replace(char oldChar, 
      char newChar) {
    //无需替换，直接返回this  
    if (oldChar == newChar){
      return this;
    }

    int len = value.length;
    int i = -1;
    /* avoid getfield opcode */
    char[] val = value; 
    //定位到需要替换的字符位置
    while (++i < len) {
      if (val[i] == oldChar) {
        break;
      }
    }
    //未找到oldChar，无需替换
    if (i >= len) {
      return this;
    } 
    //创建一个buf[]，这是关键
    //用来保存替换后的字符串
    char buf[] = new char[len];
    for (int j = 0; j < i; j++) {
      buf[j] = val[j];
    }
    while (i < len) {
      char c = val[i];
      buf[i] = (c == oldChar) ? 
        newChar : c;
      i++;
    }
    //创建一个新的字符串返回
    //原字符串不会发生任何变化
    return new String(buf, true);
  }
}
```

## 字符串常量池
