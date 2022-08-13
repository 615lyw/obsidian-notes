# 简介

Arthas 是 Alibaba 开源 Java 诊断工具。

特性：

- 在线排查问题
- 动态跟踪 Java 代码
- 实时监控 JVM 状态
- 支持 JDK 6+，支持 Linux/Mac/Windows
- Github: [https://github.com/alibaba/arthas](https://github.com/alibaba/arthas)
- 文档: [https://arthas.aliyun.com/doc/](https://arthas.aliyun.com/doc/)

# 查看函数的参数/返回值/异常信息

`watch` 命令，可以观察指定方法的 `入参` 、 `返回值` 、 `抛出异常` 、 `方法内变量值`。

用法：`watch [-b] [-e] [-f] [-s] class-pattern method-pattern express condition-express [-n value] [-x value]`

- `class-pattern` 全类名
- `method-pattern` 方法名
- `express` 观察表达式：
    - 遵循 OGNL 表达式
    - 有三个特殊字段：`returnObj` 、 `params` 、 `throwExp` 分别表示返回值、入参、抛出异常
    - 如果想同时观察多个字段，用 `{}` 包起来
- `condition-express` 条件表达式：写 Java 条件表达式，最后用单引号 `''` 包起来
- `[-n value]` ：
- `[-x value]` ：

举例：

1. 获取方法的返回值 `$ watch 全类名 方法名 returnObj`
2. 获取方法的入参 `$ watch 全类名 方法名 params`
3. 获取方法的抛出异常 `$ watch 全类名 方法名 throwExp`
4. 获取方法的入参和返回值 `$ watch 全类名 方法名 {params, returnObj}`

`params` 是一个数组，但是打印 `params` 的时候并不会把具体内容打印出来，这个时候可以使用 `-x 2` 来指定打印对象的属性遍历深度。

# 反编译代码

# JVM 监控
