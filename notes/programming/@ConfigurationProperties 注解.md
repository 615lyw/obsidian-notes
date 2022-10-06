# ConfigurationProperties 注解

作用：填充外部属性配置至 Bean 中，实现通过配置文件给组件的成员变量赋值。

使用场景：

- `@ConfigurationProperties` + `@Component` 注解到 bean 定义类上
- `@ConfigurationProperties` + `@Bean` 注解在配置类（`@Configuration`）的 bean 定义方法上
- `@ConfigurationProperties` 注解普通类然后通过 `@EnableConfigurationProperties` 把普通类定义为 bean

# 方式一

```java
@Component
//在配置文件中通过prefix+成员变量名作为key为成员变量赋值，使用@Data提供的set方法
@ConfigurationProperties(prefix="mydb")
@Data
public class FooBean {
    
}
```

# 方法二

定义一个 POJO 普通类

```java
// 这是一个一般java类,POJO,没有任何注解
public class Bean2 {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

[[@Configuration & @Bean 注解]] 可以将配置类中的一个方法标记为 bean 定义方法。在这个方法上标记 `@ConfigurationProperties` 可以将方法返回的 bean 进行配置填充。

```java
@Configuration
public class Bar {

    @Bean
    @ConfigurationProperties(prefix = "section2")
    public Bean2 bean2() {
	    // 注意，这里的 Bean2 是上面所示的一个POJO类，没有任何注解
        return new Bean2();
    }
}
```

# 方法三

`@ConfigurationProperties` 注解到一个普通 POJO 类上

```java
@ConfigurationProperties(prefix = "section3")
public class Bean3 {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

在启动类上开启扫描

```java
@SpringBootApplication
// 该注解会将类Bean3作为一个bean定义注册到bean容器，而类Bean3上的注解
// @ConfigurationProperties(prefix = "section3")会导致目标bean
// 实例的属性值使用配置文件中前缀为section3的同名属性值来填充
@EnableConfigurationProperties({Bean3.class})
public class Application {
    public static void main(String[] args) throws Exception {
        SpringApplication.run(Application.class, args);
    }
}
```

# 配置文件写法

组件成员变量实例

```java
public class DatasourceComponent {
    String driverClassName;
    String jdbcUrl;
    String username;
    String password;
    Integer maxSize;
    Boolean open;
    Map map;
    Map map2;
    List list;
    List list2;
    User user;
}
```

不管是 yml 文件还是 properties 文件都遵循：[[Spring 宽松的属性绑定规则]]

## yml 配置文件写法

```yml
# prefix=mydb
mydb:
  driver-class-name: com.mysql.jdbc.Driver
  jdbc-url: jdbc:mysql://localhost:3306/j20_db
  username: root
  password: 123456
  # maxSize 等驼峰变量名可以写成 max-size 的形式（即 Spring 宽松绑定）
  max-size: 20
  open: true
  map: {hello: thankyou, areyou: ok}
  map2:
    hello: thankyou
    areyou: ok
  list: hello, thankyou, thankyouverymuch
  list2:
    - hello
    - thanku
    - thankuverymuch
  user:
    username: songge
    password: niupi
```

```yml
# map 类型写法一：
map: {k1: v1, k2: v2}

# 写法二：
map2:
    k1, v1
    k2, v2
```

```yml
# list 类型的写法一：
list: v1, v2, v3

# 写法二：
list2:
    - v1
    - v2
    - v3
```

```yml
# javabean 写法: 直接写成员变量名即可
user:
    username: liuyiwei
    password: 123456
```

## properties 配置文件写法

```properties
mydb.driver-class-name=com.mysql.jdbc.Driver
mydb.jdbc-url=jdbc:mysql://localhost:3306/j20_db
mydb.username=root
mydb.password=123456
# map写法一：前缀+成员变量名+key=value
mydb.map.hello=thankyou
mydb.map.areyou=ok
# map写法二：前缀+成员变量名[key]=value
mydb.map2[hello]=thankyou
mydb.map2[areyou]=ok
# list写法一
mydb.list=hello, thankyou, thankyouverymuch
# list写法二
mydb.list2[0]=hello
mydb.list2[1]=thankyou
mydb.list2[2]=thankyouverymuch
# javabean写法
mydb.user.username=songge
mydb.user.password=niupi
```

# 参考

https://blog.csdn.net/andy_zhang2007/article/details/78761651
