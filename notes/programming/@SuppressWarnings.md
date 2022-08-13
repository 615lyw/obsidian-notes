# @SuppressWarnings

JDK5 引入该注解。

作用：主动忽略编译器警告。

具体能忽略的警告类型取决于编译器实现，不同的编译器支持不同的警告。可通过 `javac -X` 查看支持的非标准警告类型。

Java 规范中指出 `deprecation` 和 `unchecked` 这两种类型，任何编译器实现时必须支持。
