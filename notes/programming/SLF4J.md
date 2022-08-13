# SLF4J

SLF4J：Simple Logging Facade for Java。

![](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/LKM6Ih.png)

# 引入依赖

# 典型用法

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class HelloWorld {
    private static final Logger logger = LoggerFactory.getLogger(HelloWorld.class);

    public static void main(String[] args) {
        int a = 10;
        logger.info("hello world. a is {}", a);
    }
}
```

# 绑定日志实现框架
