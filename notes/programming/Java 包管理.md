# Java 包管理

为什么要引入包名概念？

包概念是为了解决命名冲突问题，使用 `package 包名`，给类增加前缀，使其具有唯一性，与其他包下的同名类区分开来。

用法：

- `package 包名` ：在每份源代码第一行写好，表示该类属于哪个包，必须同类所在实际目录一致
- `import 类的完全限定名` ：使用到另一个包下的类

同包的类不需要导入：当一个类中使用到其他类时，JVM 会默认去**当前包下**找这个类。
