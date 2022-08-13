# Spring 依赖配置

## 1 导包

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>${spring-version}</version>
</dependency>
```

## 2 创建配置文件

在 resources 目录下创建 `application.xml` 文件。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="<http://www.springframework.org/schema/beans>"
       xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>"
       xmlns:context="<http://www.springframework.org/schema/context>"
       xmlns:aop="<http://www.springframework.org/schema/aop>"
       xmlns:tx="<http://www.springframework.org/schema/tx>"
       xsi:schemaLocation="
        <http://www.springframework.org/schema/beans> <https://www.springframework.org/schema/beans/spring-beans.xsd>
        <http://www.springframework.org/schema/aop> <https://www.springframework.org/schema/aop/spring-aop.xsd>
        <http://www.springframework.org/schema/tx> <https://www.springframework.org/schema/tx/spring-tx.xsd>
        <http://www.springframework.org/schema/context> <https://www.springframework.org/schema/context/spring-context.xsd>">

    <!-- bean definitions here -->
    
    
</beans>
```

[[控制反转 IoC]]

## bean 工厂

我们可以把 Spring 理解为一个 beanFactory，负责创建并初始化各种各样的 bean，而应用上下文 ApplicationContext 在 beanFactory 的基础上进行了进一步的封装。

ApplicationContext 是一个接口，有两个实现类：

1. ClassPathXmlApplicationContext ("读取 classpath 下的配置文件")

```java
//需要加载spring配置文件 → 直接写 resources（classpath）下的配置文件名
ApplicationContext applicationContext = 
    new ClassPathXmlApplicationContext("application.xml");
```

2. FileSystemXmlApplicationContext ("文件系统路径下的配置文件")

```java
String configLocation = "D:\\WorkSpace\\j20_workspace\\codes\\day01-springioc\\demo3_ioc2\\src\\main\\resources\\application.xml";
ApplicationContext applicationContext = 
    new FileSystemXmlApplicationContext(configLocation);
```

# 管理 bean

## 注册 bean

id 属性：bean 在容器中的唯一标识

class 属性：组件的全类名

```xml
<bean id="userService" class="com.cskaoyan.service.UserServiceImpl">
```

## 依赖注入

客户端依赖某个成员变量，通过 setter 或构造器完成依赖对象赋值。

待依赖注入示例对象：

```java
public class UserServiceImpl {
    private String name;
    private Integer age;
}
```

### 通过 setter 注入

通过 `property` 标签。

```xml
<bean id="userService" class="com.cskaoyan.service.UserServiceImpl">
	// 实现机制：fieldName → setFiledName(), 与实例变量名或 set 方法的参数名无关
	<property name="fieldName" ref="anotherBeanId"/>
	// "numberValue"会由 spring 自动转换为成员变量的数值变量类型
	<property name="fieldName" value="stringValue or numberValue"/>
</bean>
```

### 通过有参构造器注入

```xml
<bean id="userService" class="com.cskaoyan.service.UserServiceImpl">
	// name 值对应有参构造器的形参名
    <constructor-arg name="name" value="liuyiwei"/>
    <constructor-arg name="age" value="1"/>
</bean>
```

## 设置作用域 scope

scope：

- singleton（**默认**）：单例
- prototype：非单例

```xml
<bean id="userService" class="com.cskaoyan.service.UserServiceImpl" scope="singleton"/>
```

## 设置第一优先级

primary 属性：根据接口类型获取时，提供第一优先级

```xml
<bean id="userService" class="com.cskaoyan.service.UserServiceImpl" primary="true">
```

# 获取 bean

## 通过组件 id 获取

```java
//需要加载spring配置文件 → 直接写 resources（classpath）下的配置文件名
ApplicationContext applicationContext = 
    new ClassPathXmlApplicationContext("application.xml");
//根据组件在容器中的id获取组件实例
UserService userService = (UserService) applicationContext.getBean("userService");
```

## 通过组件类型获取

- 按照接口类型获取：需要保证组件在容器中的唯一性或首要性
- 按照实例类型获取

```java
ApplicationContext applicationContext = 
    new ClassPathXmlApplicationContext("application.xml");
//class指的是类型 
UserService userService = applicationContext.getBean(UserService.class);
```

如果某个类型的组件实例不唯一，则会报错：NoUniqueBeanDefinitionException。

```xml
<bean id="userService" class="com.cskaoyan.service.UserServiceImpl"/>

<!--这个类型的组件现在不唯一-->
<bean id="userService2" class="com.cskaoyan.service.UserServiceImpl"/>
```

# bean 的生命周期

即 Spring IoC 初始化 bean 以及销毁 bean 的过程。

大致过程：

1. bean 的初始化
   1. 资源定位
   2. 解析获得 bean 定义
   3. 发布 bean 定义
   4. 实例化 bean + 依赖注入（取决于 XML 中怎么实例化 bean）：
      - 调用无参构造器 + setter 方法
      - 调用有参构造器

```java
   // 通过无参构造器 + setter方法完成实例化 bean 和依赖注入
   <bean id="userService" class="com.cskaoyan.service.UserServiceImpl">
   	<property name="fieldName" ref="anotherBeanId"/>
   	<property name="fieldName" value="stringValue or numberValue"/>
   </bean>
```

```java
   // 通过有参构造器同时完成 bean 的实例化和依赖注入
   <bean id="user" class="com.cskaoyan.bean.User">
       <constructor-arg name="usernamea" value="admin"/>
       <constructor-arg name="passwordb" value="admin123"/>
   </bean>
```

2. bean 的个性化定制

   1. setBeanName
   2. setBeanFactory
   3. setApplicationContext
   4. @PostConstruct 标注的自定义初始化方法

3. bean 的销毁

Spring 工厂实例化组件的顺序取决于在 XML 配置文件中组件的注册顺序。

![](https://gitee.com/ryan615/PicGoBed/raw/master/image/20201101211119.png)

只要组件实现了对应接口，就会经历生命周期的那个阶段：

```java
public class LifeCycleBean implements BeanNameAware, BeanFactoryAware,
        ApplicationContextAware, InitializingBean, DisposableBean {

    String param;

    public LifeCycleBean() {
        System.out.println("1、init");
    }

    public String getParam() {
        return param;
    }

    public void setParam(String param) {
        System.out.println("2、set方法");
        this.param = param;
    }

    @Override
    public void setBeanName(String s) {
        System.out.println("3、BeanNameAware的setBeanName ： " + s);
    }

    @Override
    public void setBeanFactory(BeanFactory beanFactory) throws BeansException {
        System.out.println("4、BeanFactoryAware");
    }

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        System.out.println("5、ApplicationContextAware");
    }

    @Override
    public void afterPropertiesSet() throws Exception {
        System.out.println("7 InitializingBean");
    }

    @PostConstruct
    public void myinit(){
        System.out.println("8 custom init");
    }

    @Override
    public void destroy() throws Exception {
        System.out.println("10 DisposableBean");
    }

    public void mydestory(){
        System.out.println("11 custom destory");
    }
}
```

# AOP 面向切面编程

# 装配 Bean

- SpringBoot AutoConfiguration 自动装配
- JavaConfig
- XML

Spring 应用上下文中所有的 bean 都会给定一个 id。只要当我们没有明确地为 bean 设置 id，Spring 会根据类名为其指定一个 id ，即将类名的第一个字母变为小写。

## XML 配置

**组件注册**

1. 通过无参构造

```xml
<bean id="userService" class="com.cskaoyan.service.UserServiceImpl"/>
```

2. 通过有参构造器

```xml
<bean id="user" class="com.cskaoyan.bean.User">
    <!-- name 表示有参构造接受的形参名 -->
    <constructor-arg name="usernamea" value="admin"/>
    <constructor-arg name="passwordb" value="admin123"/>
</bean>
```

**依赖注入**

通过 `<property>` 标签完成依赖注入：

```xml
<!--注册dao-->
<bean id="userDao" class="com.cskaoyan.dao.UserDaoImpl"/>

<!--注册service-->
<bean id="userService" class="com.cskaoyan.service.UserServiceImpl">
    <!--维护service和dao之间的关系-->
    <!--property标签中的name属性值是和set方法对应的-->
    <!--看到ref，就要从容器中找对应的组件id-->
    <property name="userDao" ref="userDao"/>
</bean>
```

## JavaConfig 配置

## 自动装配

通过 `@Component` 声明一个 bean 组件：

```java
@Component
public class SgtPeppers implements CompactDisc {
  private String title = "Sgt. Pepper's Lonely Hearts Club Band";  
  private String artist = "The Beatles";
  
  public void play() {
    System.out.println("Playing " + title + " by " + artist);
  } 
}
```

组件扫描默认是不启用的，有两种方式开启：

- JavaConfig
- xml 文件

**通过 JavaConfig 开启组件扫描：**

通过 `@Configuration` 声明这是一个配置类，等效于 application.xml 文件

通过 `@ComponentScan` 开启组件扫描，默认扫描配置类所在包（soundsystem）以及其下的子包，查找带有 `@Component` 注解的类将其视为 Bean

也可以通过 `@ComponentScan(basePackages="com.liuyiwei")` 配置扫描范围

```java
package soundsystem;

@Configuration
@ComponentScan
public class CDPlayerConfig { 
}
```

**通过 XML 文件开启组件扫描：**

```xml
<context:component-scan base-package="soundsystem"/>
```
