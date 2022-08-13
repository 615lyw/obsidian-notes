# ClassNotFoundException

该类属于 Exception，属于可恢复错误。

代码中没有直接使用类名，而是通过 `Class.forName (String name)` 等类名字符串来获取类，如果此时没有在 classpath 中找到该类，则抛出 ClassNotFoundException 异常。

# NoClassDefFoundError

注意该类属于 Error，属于不可恢复错误。

代码中直接使用到类名，通过了编译，在运行时，该类的 class 文件并不在 classpath 中，便会抛出 NoClassDefFoundError。
