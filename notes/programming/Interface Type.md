Type interface 代表 Java 中所有类型：

- Class
- GenericArrayType
- ParameterizedType
- TypeVariable
- WildcardType

Class 类：代表类和接口、枚举类型（属于类），注解（属于接口）。

参数化类型 ParameterizedType：把类型定义为参数（产生类型形参和类型实参）。
例子： 

- `HashMap<String, List<String>>`
- `Class<?>`
- `HashMap<String, List<? extends Person>>`
- Integer、String、double 等不是参数化类型

TypeVariable 类型变量，即常用的 T，K 这种泛型变量，例如：`Map<K, V>`。

GenericArrayType 范型数组类型，代表包含 ParameterizedType 或 TypeVariable 的元素列表，例如：`T[] value`, `List<String>[] list`。

WildcardType 通配符类型，例如：` <?>`, `<? extends Number>`。

**为什么引入 Type 接口？**

按理来说，应该让 Class 类提供范型信息，但是 Class 类代表字节码，修改 Class 类，字节码也需要修改，JVM 指令也需要修改，JVM 指令修改很致命。

JVM 指令不能修改，故只能新建类以支持范型信息以及在编译时支持范型，运行时擦除。

## 参考

- [Java中的Type类型详解 - 掘金](https://juejin.cn/post/6844903597977632776)
- [Type Inference (The Java™ Tutorials > Learning the Java Language > Generics (Updated))](https://docs.oracle.com/javase/tutorial/java/generics/genTypeInference.html)
- [秒懂Java类型（Type）系统 - 知乎](https://zhuanlan.zhihu.com/p/64584427)