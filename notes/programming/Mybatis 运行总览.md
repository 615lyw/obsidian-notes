# Mybatis 运行总览

1. Mybatis 初始化阶段：解析配置文件，初始化 SqlSessionFactory
2. 数据库读写阶段：通过 SqlSessionFactory 获取 SqlSession 读写数据库

## 初始化阶段

在项目启动时完成 Mybatis 初始化阶段：

1. 通过 Resources 类获取配置文件流（[[如何读取资源文件]]）
2. 解析配置文件流生成 Configuration 对象
	1. 通过 XMLConfigBuilder 逐层解析 XML 格式配置文件各元素节点
	2. 把解析结果存入 Configuration 对象
3. 通过 Configuration 对象生成一个 DefaultSqlSessionFactory（SqlSessionFactory 接口实例）

## 数据库读写阶段

### 获取 SqlSession

`SqlSession session = sqlSessionFactory.openSession()`

1. 一个 SqlSession 对象可供多次数据库读写访问
2. DefaultSqlSession（SqlSession 接口实例对象）支持 CRUD、事务操作

### 生成映射接口的实现对象

`UserMapper userMapper = session.getMapper(UserMapper.class);`

`userMapper.queryUserBySchoolName(userParam)`  会被代理对象拦截。（[[动态代理]]）

### 将 XML 文件中的数据库操作语句转换为标准 SQL 语句

### 执行数据库查询

先查缓存（[[Mybatis 缓存]]），缓存没命中则查询数据库，将数据库返回结果放入缓存。

### 处理结果集

将数据库记录转换为所需对象。

- 生成输出对象
- 根据自动映射或用户设定的属性映射给输出对象属性
