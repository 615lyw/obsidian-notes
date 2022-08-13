# Logback

因为和 [[SLF4J]] 是同一作者，故实现了 slf4j-api。

logback-core, logback-classic and logback-access

Logger 之间存在 **Named Hierarchy** 关系，有一个 rootLogger

Logger assigned level 和 effective level 以及 logging request level（日志打印级别）

**the basic selection rule**

用所在类的完全限定名命名 Logger

an output destination is called an appender.
More than one appender can be attached to a logger.

**Appender Additivity**

**associating a layout with an appender**

The layout is responsible for formatting the logging request


Logger Appender Layout 三者关系

一个 Logger 可以关联多个 Appender，一个 Appender 只能配置一个 Layout。

```java
logger.debug("The new entry is "+entry+".");
logger.debug("The new entry is {}.", entry);
```


后者性能更好，原因：



# 参考

[Logback 官网](https://logback.qos.ch/)