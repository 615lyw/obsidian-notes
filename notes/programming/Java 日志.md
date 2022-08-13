# 问题

- 日志框架众多，不同的类库可能使用不同的日志框架容易导致冲突 [[如何解决日志框架冲突]]
- 日志配置复杂且容易出错
- 日志记录本身使用问题：比如胡乱使用日志级别

# 日志框架

日志框架分为两类：

- 日志门面/抽象
    - 定义日志打印抽象接口，不负责具体实现
    - [[SLF4J]]
    - `JCL` （Jakarta Commons Logging），即 `commons-logging-xx.jar`
- 日志实现
    - 负责指定日志打印到哪，打印格式如何，是否为异步打印等
    - 需要引入配置文件才能打印出日志
    - [[Log4j|Log4j and Log4j2]]
    - [[logback]]
    - `JUL` （ `java.util.logging` ）

## Java 日志框架发展历史

> - log4j 是 Java 社区最早的日志框架，推出后一度成为 Java 的事实日志标准，据说 Apache 曾建议 Sun 把 log4j 加入到 Java 标准库中，但是被 Sun 拒绝
> - 在 Java1.4 中，Sun 在标准库中推出了自己的日志框架 java.util.logging，功能相对简陋
> - 虽然 JUL 相对简陋，但还是有类库采用了它，这就出现了同一个项目中同时使用 log4j 和 JUL 要维护两套配置的问题，Apache 试图解决这个问题，推出了 JCL 日志门面（接口），定义了一套日志接口，底层实现支持 log4j 和 JUL，但是并没有解决多套配置的问题
> - log4j 的主力开发 Ceki Gülcü 由于某些原因离开了 Apache，创建了 slf4j 日志门面（接口），并实现了性能比 log4j 性能更好的 logback（如果 Ceki Gülcü 没有离开 Apache，这应该就是 log4j2 的 codebase 了）
> - Apache 不甘示弱，成立了不兼容 log4j 1.x 的 log4j2 项目，引入了 logback 的特性（还酸酸地说解决了 logback 架构上存在的问题），但目前采用率不是很高

# 最佳实践

SDK 项目

- 使用日志门面（比如：SLF4J）API
- 为了调试需要引入一个日志实现，但不要提交到类库中

新项目

- 使用日志抽象+日志实现提高灵活性，便于以后更换日志实现

[[日志最佳实践]]

# 参考

- [Java 日志体系总结](https://albenw.github.io/posts/854fc091/)
- [Java 的那些日志框架们](http://tunzao.me/articles/java-logging/)
