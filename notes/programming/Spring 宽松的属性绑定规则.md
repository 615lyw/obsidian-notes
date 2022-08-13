# Spring 宽松的属性绑定规则

绑定外部属性方式：

- [[@Value 注解]]
- [[@ConfigurationProperties 注解]]

以下变体都将绑定到 hostName 属性上

```properties
mail.hostName=localhost
mail.hostname=localhost
mail.host_name=localhost
mail.host-name=localhost
mail.HOST_NAME=localhost
```

```yml
mail:
  hostName: localhost
  hostname: localhost
  host_name: localhost
  host-name: localhost
  HOST_NAME: localhost
```
