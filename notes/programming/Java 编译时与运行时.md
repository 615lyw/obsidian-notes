---
date created: 2022-01-10, 20:29:22
date modified: 2022-06-20, 16:48:39
---

# Java 编译执行过程

Java 源文件 -> javac 编译 -> class 文件 -> java 启动 JVM 执行 class 文件。

class 是字节码文件，字节码是 JVM 的指令，JVM 再把字节码转换为目标机器的机器指令。

# java、javac 与 CLASSPATH

编译和运行时都需要通过 CLASSPATH 指定 class 文件的位置。

jar 包实际只是一个压缩文件，里面包含许多 class 文件，故通过 CLASSPATH 像指定文件夹一样直接引入 jar 包即可。
