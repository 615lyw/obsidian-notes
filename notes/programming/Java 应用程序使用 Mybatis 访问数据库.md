[官方配置文档](https://mybatis.org/mybatis-3/zh/getting-started.html)

# 导包

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.47</version>
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.4</version>
</dependency>
```

# 通过 XML 构建 SqlSessionFactoryBuilder

在 resources 目录（**类路径**）下新建 mybatis.xml 配置文件。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="dev">
        <environment id="dev">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/day08?useSSL=true"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
		<environment id="prod">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/day08?useSSL=true"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <package name="com.liuyiwei.mapper"/>
    </mappers>
</configuration>
```

```
String resource = "org/mybatis/example/mybatis-config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource); SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
```

# 从 SqlSessionFactory 中获取 SqlSession&通过 SqlSession 获得映射接口

```
try (SqlSession session = sqlSessionFactory.openSession()) { 
	BlogMapper mapper = session.getMapper(BlogMapper.class);
	Blog blog = mapper.selectBlog(101); 
}
```

# 建立 Mapper XML 文件和映射接口

注意：映射文件和映射接口文件的分别在 resources 文件夹和 java 文件夹的同一路径下。

# 生命周期

注意官方文档最后生命周期部分。
