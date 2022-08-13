# Mybatis

- 持久层框架，ORM 框架
- 旧名称：ibatis
- 避免了编写含有大量模板代码的 JDBC 代码

传统访问数据库是通过 [[JDBC]] 和数据库驱动程序来实现，步骤：

1. 注册驱动程序
2. 建立连接
3. 创建执行平台
4. 拼装 SQL 并执行：需要解析对象将对象属性作为参数传入 SQL 
5. 关闭连接
6. 处理结果集转换为 Java 对象：需要把 SQL 返回结果转换为对象

ORM 框架主要目的就是实现步骤 4、6 自动化。

# 核心功能

1. 映射接口（类似 UserMapper.java）文件中抽象方法和映射文件（UserMapper.xml）中的 SQL 语句绑定，调用抽象方法即执行对应的 SQL 语句
2. SQL 语句的参数和返回结果与抽象方法的入参、出参的 Java 对象互相映射
3. 映射文件中的特殊标签解析为纯 SQL 语句

# 使用

# 映射器接口中的方法与 SQL 语句之间的映射

## 通过 XML 映射文件实现 SQL 映射

在 resources 目录下配置 UserMapper. xml 映射文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="mapper接口全类名">
    <select id="" resultType="">
        具体的SQL语句
    </select>

    <insert id="">
        具体的SQL语句
    </insert>

    <delete id="">
        具体的SQL语句
    </delete>

    <update id="">
        具体的SQL语句
    </update>
</mapper>
```

### 通过注解实现 SQL 语句映射

```java
package org.mybatis.example;

public interface BlogMapper {
  @Select("SELECT * FROM blog WHERE id = #{id}")
  Blog selectBlog(int id);
}
```

#  XML 映射文件

顶级元素类别：
- `cache` – 该命名空间的缓存配置。
- `cache-ref` – 引用其它命名空间的缓存配置。
- `resultMap` – 描述如何从数据库结果集中加载对象，是最复杂也是最强大的元素。~
- ~~ `parameterMap` – 老式风格的参数映射。此元素已被废弃，并可能在将来被移除！请使用行内参数映射。文档中不会介绍此元素。~~
- `sql` – 可被其它语句引用的可重用语句块。
- `insert` – 映射插入语句。
- `update` – 映射更新语句。
- `delete` – 映射删除语句。
- `select` – 映射查询语句。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="mapper接口全类名">
    <select id="" resultType="">
        具体的SQL语句
    </select>

    <insert id="">
        具体的SQL语句
    </insert>

    <delete id="">
        具体的SQL语句
    </delete>

    <update id="">
        具体的SQL语句
    </update>
</mapper>
```

- mapper 元素的 namespace 属性表示整个 Mybatis 环境下该**映射文件的唯一标识**
- curd 四种操作对应映射文件里的四个标签 `<select>` 、 `<update>` 、 `<delete>` 、 `<insert>`
- 标签的 id 属性在同一个映射文件下不能重复，在不同的映射文件中可以有相同的 id（namespace + id 保证每个标签在 Mybatis 环境下是全局唯一的）

## select

```xml
<select id="selectPerson" parameterType="int" resultType="hashmap">
  SELECT * FROM PERSON WHERE ID = #{id}
</select>
```

select 标签的 resultType 写法

`<select>` 标签必须要有 resultType 属性，**表示让 Mybatis 将 SQL 语句的返回值封装成什么类型**

所需封装后的类型为：
- 基本数据类型（或包装类型）或 String，则 resultType="纯小写"（正常写或纯小写都行，通常纯小写）
- Javabean 类型，则 resultType=" Javabean 的全类名"

`<update>` 、 `<delete>` 、 `<insert>` 标签没有 resultType 属性，实际返回值类型有两种可选：int（常用）、boolean 以及两者的包装类
- 当 int 为 0 或 boolean 为 false 时表示操作失败
- 当 int 为 1 或 boolean 为 true 时表示操作成功

SQL 参数占位符写法

标签内的 SQL 语句用一个占位符表示所需参数：

形式：`#{名称}`

根据 **CURD API 的传参类型**来决定占位符中的名称写法：（细节可见 CURD API 的使用）
- 如果传的是基本数据类型或 java. lang 包下的类，名称可以随便写，通常写成形参名
- 如果传的是 Javabean 类型，写 Javabean 的成员变量名（实际上是调用 Javabean 的 get 方法）
- 如果传的是 map 类型，写 map 中的 key 名称

# mybatis. xml 配置文件标签

MyBatis 配置文件包含了影响 MyBatis 行为的设置和属性信息。

## 必需标签

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/day08?useSSL=true"/>
                <property name="username" value="root"/>
                <property name="password" value=""/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        配置映射文件
    </mappers>
</configuration>
```

## 可选标签

注意要按照各标签特定的顺序书写。

### properties 标签

可用来读取 classpath 下的 properties 配置文件中的值或作为 mybatis. xml 中的全局变量键值对。

在 classpath 下有 db. properties 文件，内容为：

```properties
db.driver=com.mysql.jdbc.Driver
db.jdbcUrl=jdbc:mysql://localhost:3306/j20_db?useUnicode=true&characterEncoding=utf-8
db.username=root
db.password=123456
```

写法一：读取 classpath 下的 properties 配置文件中的值

`<properties resource="加载classpath下的 db.properties 配置文件">`

通过 `${配置文件中的 key}` 完成在 mybatis. xml 中读取配置文件的值。

写法二：作为 mybatis. xml 中的全局变量键值对

```
<properties resource=“”>
	<property name="key" value="val">
</properties>
```

写法一、二综合：

```xml
<properties resource="db.properties">
    <property name="db.driver" value="com.mysql.jdbc.Driver"/>
    <property name="db.jdbcUrl" value="jdbc:mysql://localhost:3306/j20_db?useUnicode=true&amp;characterEncoding=utf-8"/>
</properties>

<environments default="development">
    <environment id="development">
        <transactionManager type="JDBC"/>
        <dataSource type="POOLED">
            <property name="driver" value="${db.driver}"/>
            <property name="url" value="${db.jdbcUrl}"/>
            <property name="username" value="${db.username}"/>
            <property name="password" value="${db.password}"/>
        </dataSource>
    </environment>
</environments>
```

### settings 标签

mybatis 的配置：缓存和懒加载

### typeAliases 标签

用于配置 resultType 中各类型对应的别名，意在降低冗余的全类名书写。

- 默认：基本类型、java. lang 包目录下的范围内的直接写成小写，例如 Integer → integer，String → string 等
- 自定义：针对性配置、批量配置

**给特定的 Javabean 配置别名**

```xml
<typeAliases>
	<typeAlias type="com.cskaoyan.bean.User" alias="user"/>
</typeAliases>
```

**批量配置别名**

别名规则：所配置的包目录下的类名以**纯小写**形式作为别名，例如 UserDetail → userdetail

```xml
<typeAliases>
	<package name="com.cskaoyan.bean"/>
</typeAliases>
```

**如果不想配置别名直接写全类名即可。**

### typeHandler 标签

在输入映射和输出映射过程中完成：Javabean 和 JsonString 之间的转换。

类似对象序列化/反序列化的任务，即把一个 java 对象以 json 字符串的形式存储在数据库的一个列下或反过来。

jackSon 的用法：

```java
ObjectMapper objectMapper = new ObjectMapper();
User user = new User("小66", "1234", null);
// 传入一个待json字符串化的对象
String s = objectMapper.writeValueAsString(user);

// 根据json字符串和Javabean类型生成一个对应Javabean的对象
User user1 = objectMapper.readValue(s, User.class);
```

需要我们自己实现一个 typeHandler 类完成 Object 和 String 之间的双向转换，使用 Gson 或 jackSon 都可以。

**自己实现一个 typeHandler**

注意点：

1. `@MappedTypes()` ：用于省略在映射文件输入/输出映射中重复写自己实现的 typeHandler，使用该注解后 Mybatis 会自动检测输入/输出映射过程中涉及到 AnotherBean 类型时来执行此 typeHandler 代码。

2. 实现 TypeHandler 接口的 4 个方法，泛型填 Javabean 类型
3. 回忆 JDBC 的 PreparedStatement 的 setString 以及 ResultSet 的 getString

```java
@MappedTypes(AnotherBean.class)
public class AnotherBeanTypeHandler implements TypeHandler<AnotherBean> {
    ObjectMapper objectMapper = new ObjectMapper();
    
    //输入映射时用于给PreparedStatement(?,?,?) 中的 ？填充内容，故调用setString()
    @SneakyThrows
    @Override
    public void setParameter(PreparedStatement preparedStatement, int index, AnotherBean anotherBean, JdbcType jdbcType) throws SQLException {
        String s = objectMapper.writeValueAsString(anotherBean);
        preparedStatement.setString(index, s);
    }
    
    // 从结果集中根据列名获得json字符串，再转换成目标对象类型返回
    @Override
    public AnotherBean getResult(ResultSet resultSet, String columnName) throws SQLException {
        String json = resultSet.getString(columnName);
        return transfer(json);
    }
    
    // 按照列下标从结果集中取出字符串 
    @Override
    public AnotherBean getResult(ResultSet resultSet, int index) throws SQLException {
        //获得json字符串
        String json = resultSet.getString(index);
        return transfer(json);
    }
    
    // CallableStatement 与存储过程(一系列SQL语句集合,类似批处理文件)有关
    @Override
    public AnotherBean getResult(CallableStatement callableStatement, int index) throws SQLException {
        //获得json字符串
        String json = callableStatement.getString(index);
        return transfer(json);
    }

    // 转换
    private AnotherBean transfer(String json){
        AnotherBean anotherBean = null;
        if (null == json){
            return null;
        }
        try {
            anotherBean = objectMapper.readValue(json, AnotherBean.class);
        } catch (JsonProcessingException e) {
            e.printStackTrace();
        }
        return anotherBean;
    }
}
```

**将实现的 typeHandler 在 mybatis. xml 中进行配置**

```xml
<typeHandlers>
    <typeHandler handler="com.liuyiwei.typehandler.JavaBean2Json"/>
    <!--<package name="com.liuyiwei.typehandler"/>-->
</typeHandlers>
```

### plugins

mybatis-pagehelper 分页插件

### mappers 标签

告诉 MyBatis 到哪里去找映射文件。

`<mapper resourse="classpath下的映射文件"/>`

```xml
<!-- 使用相对于classpath的资源引用 -->
<mappers>
  <mapper resource="org/mybatis/builder/AuthorMapper.xml"/>
  <mapper resource="org/mybatis/builder/BlogMapper.xml"/>
  <mapper resource="org/mybatis/builder/PostMapper.xml"/>
</mappers>
```

`<mapper url="file:///映射文件的文件系统路径"/>`

```xml
<!-- 使用完全限定资源定位符（URL） -->
<mappers>
  <mapper url="file:///var/mappers/AuthorMapper.xml"/>
  <mapper url="file:///var/mappers/BlogMapper.xml"/>
  <mapper url="file:///var/mappers/PostMapper.xml"/>
</mappers>
```

**【后期常规用法，重要】**

映射器接口即以前的 Dao 接口，映射文件就是以前的 DaoImpl。

```xml
<!-- 使用映射器接口的完全限定类名 -->
<mappers>
  <mapper class="org.mybatis.builder.AuthorMapper"/>
  <mapper class="org.mybatis.builder.BlogMapper"/>
  <mapper class="org.mybatis.builder.PostMapper"/>
</mappers>
```

批量注册：`<package name="接口和映射文件所在的包">`

```xml
<!-- 将包内的映射器接口全部注册为映射器 -->
<mappers>
  <package name="org.mybatis.builder"/>
</mappers>
```

映射器接口和映射文件对应关系：

- 一个接口对应一个映射文件
- 接口的全类名和映射文件的命名空间一致 → namespace 写成接口的全类名
- 接口中的方法名和映射文件中的标签的 id 一致 → id 写成方法的名字
- namespace+id 可以找到唯一对应的 sql 语句 → 通过接口和方法也可以找到唯一对应的 sql
- 接口和映射文件编译后在同一级目录下，并且同名

<img src="https://gitee.com/ryan615/PicGoBed/raw/master/image/20200522080619.png" width=300px/>

【注意】

1. 映射器接口中不允许方法重载，因为一个方法对应的就是映射文件中的一个标签，id 必须唯一
2. 接口中的方法一定要有对应的标签
3. 标签可以多写，接口中的方法不能多写

# 输入映射

映射器接口方法的形参和映射文件中 SQL 语句所需参数之前的映射规则。

主要关注 `#{}` 中的值写法。

## 单个参数

接口中的方法只有一个形参。

举例：

- login (int id)：值类型
- login (String username)：值类型
- login (User user)：Javabean 对象
- login (Map map)：map 对象

形参类型：

- 值类型：基本类型或 java. lang 包下的类：`#{名称}`  名称随便写，通常和形参名保持一致
- Javabean 对象或 map 对象：`#{名称}` 名称写 Javabean 的成员变量名或 map 的 key

## 多个参数

接口中的方法有多个形参。

举例：

- login (String username, String password)
- login (User user1, Map userMap2)

形参类型 → `#{名称}` ：

- 值类型：基本类型或 java. lang 包下的类：根据参数的顺序写 param 序号（从 1 开始编号），arg 序号（从 0 开始）
- Javabean 对象或 map 对象： 使用 param 序号+成员变量名或 key，`#{param1.username}`，`#{param2.key}`
- 混合类型（基本类型、Javabean、map）
    - 基本类型：只有 param 或 arg 序号
    - Javabean：`#{param.成员变量名}` 或 `#{arg.成员变量名}`
    - map：`#{param.key}` 或 `#{arg.key}`

## 使用注解完成输入映射

不用管形参有几个、是什么类型

在方法的形参前加上 `@Param("任意名称，用于占位符")`

原则：所见即所用

形参：

- 值类型：占位符 `#{@Param中的名称}`
- Javabean 或 map：占位符 `#{@Param中的名称.key 或成员变量名}`

# 输出映射

主要用于 select 的查询结果的封装。

**注意⚠️：不管是单个查询结果还是多个查询结果的封装，resultType 都是写单个值类型，区别只在于映射接口中方法的返回值。**

## 基本类型

单个结果的封装：

select 结果封装为 int 或 String 类型。

注意：select 只能查询单列，如果查询多列会报错。【待 Mybatis 和 Spring 整合时验证】

当 select 结果为单个整数时最好用包装类接受，因为如果没有查询到结果，会返回 null。

多个结果的封装：

只用修改映射器接口中的方法的返回值，映射文件的 **resultType 写单个值类型即可**（不要写成 list）。

返回值写成：`String[]` 或 `List<String>`

## Javabean

将查询结果封装为一个 user 对象。

封装原理：查询结果的列名与 user 的成员变量名对应，Mybatis 通过 set 方法实现封装 Javabean。

查询结果的列名和数据库表的列名不是一回事，因为可以使用 as。当表的列名和 Javabean 的成员变量不一致时，可以用 as 进行转换。

之前的 resultType 写法是 Javabean 的全类名。

## resultMap

resultType 可以替换为 resultMap，主要针对 Javabean 使用。

resultMap 做的是映射：查询结果的列名 → Javabean 的成员变量名。

写法：

1. 引入 resultMap 标签（ id 属性表示该标签在命名空间中的唯一标识； type 属性表示记录的封装类型）
    1. 写 result 标签（column: 查询结果的列名；property: Javabean 的成员变量名）
    2. 如果是主键可以使用 id 标签，用法同 result 标签
    3. 通常 Javabean 每个成员变量都需要完成映射，即使成员变量名和查询结果的列名是一样的

好处：可以多处引用同一份 resultMap，不用在每个具体的 SQL 语句出进行 as 别名转换。

案例：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.example.demo.mapper.UserMapper">
    
    <resultMap id="baseResultMap" type="com.example.demo.bean.User">
        <id column="id" jdbcType="INTEGER" property="id"/>
        <result column="username" jdbcType="VARCHAR" property="username"/>
        <result column="password" jdbcType="VARCHAR" property="password"/>
    </resultMap>
    
</mapper>
```

# 动态 SQL

根据条件不同动态改变预编译的 SQL 语句。

## where

之前 SQL 语句使用 where 的部分都可以替换为 where 标签。

where 标签可以去掉后面直接跟的 and 或 or，避免 SQL 语法错误，故每个条件子句均可以 and 开头。

```xml
<where>
	条件子句
</where>
```

## if

注意 test 属性中条件的写法和 SQL 语句中条件的写法不是一回事。

### test 条件写法

test 可用的值：方法形参上的 @Param ("值")

```xml
<if test="username != null">
    具体的 sql 语句
</if>
```

各条件写法：

- 判空：== null
- 判非空：!= null
- 字符串（用单引号包含）：== 'zifuchuan'
- 大于：age gt 20
- 小于：age lt 30
- 多个条件通过 and 或 or 进行连接
- 大于等于：age gt 15 or age == 15
- 小于等于：age lt 15 or age == 15
- 字符串长度：'zifuchuan'. length

### XML 中条件写法

XML 文件中的大于、小于写法，即映射文件中 SQL 语句条件写法：

- 大于：`&gt;`
- 小于：`&lt;`
- 大于等于：`&gt;=`
- 小于等于：`&lt;=`

## choose-when-otherwise

即 if-else 结构。

test 条件写法同 if

```xml
<choose>
	<when test="age gt 15">
    	age &gt; 15
    </when>
    <otherwise>
    	age &lt;= 15
    </otherwise>
</choose>
```

## trim

```xml
<trim prefix="" suffix="" prefixOverrides="" suffixOverrides=""></trim>
```

prefix="xxx"：在最前面拼接 xxx

suffix="xxx"：在最后面拼接 xxx

prefixOverrides="xxx"：如果最前面存在 xxx，则去掉

suffixOverrides="xxx"：如果最后面存在 xxx，则去掉

先执行 prefixOverrides 和 suffixOverrides 去除多余的内容，再执行 prefix 和 suffix。

## sql-include

SQL 语句片段的引用。

用 sql 标签提取映射文件中重复的 SQL 语句片段，用 include 标签完成 sql 标签的引用。

`<sql id=""></sql>`

`<include refid="SQL标签的id"></include>`

常用场景：

1. 提取重复的查询结果的列

```sql
select username, password, age, gender from j20_user_t
```

```xml
<sql id="foo">username, password, age, gender </sql>

select <include refid="foo"/> from j20_user_t
```

2. 提取重复的 where 条件

标签也可以放入 sql 标签中

```xml
<sql id="bar">
	<where>
    	条件
    </where>
</sql>
```

## set

用于 update。

`<set></set>`

作用：完成对拼接语句末尾多余的逗号的处理。

## foreach

当映射器接口中的方法的形参为数组或列表时，可以使用 foreach 来遍历。

场景：

```sql
select id, username, password from j20_user_t where id in (?, ?, ?, ?);
```

通过 separator 完成每个 ? 之间的分割符

通过 open 和 close 完成 () 的拼接

没有使用 `@Param` 时的写法：

- 形参为 `List` 类型时，collection 项固定写为 `list`
- 形参为 `array` 类型时，collection 项固定写为 `array`

```xml
<where>
    id in
	<foreach collection="list" item="user" open="(" separator="," close=")">
    	#{user.id}
  </foreach>
</where>
```

使用 `@Param` 时的写法：“所见即所用”，collection 写为 `@Param` 中的值，不管形参是 `List` 还是 `array`。

## selectKey

在某条 SQL 语句执行前或后，额外执行一条 SQL 语句。

`<selectKey>` 写在某个  CRUD  标签里。

常用场景 1：

通常插入语句自增主键不填，由数据库自己给定，故在 Javabean 里自增主键值为 null，搭配 `select last_insert_id()` 查询最近插入记录的自增主键值。

法一：

keyProperty：给 Javabean 哪个成员变量赋值

keyColumn：查询结结果的列名

order：相对下面 insert 语句的顺序

resultType：selectKey 语句查询结果封装类型

```xml
<insert>
	<selectKey keyProperty="user.id" keyColumn="idz" order="before/after" resultType="integer">
	select last_insert_id() as idz
	</selectKey>
	insert into table_name () values ()
</insert>
```

法二：

使用 useGeneratedKeys 属性获得自增的主键

`<insert id useGeneratedKeys="true" keyProperty="user.id">`

相比 selectKey 的好处：如果一次插入多条数据，可以获得多条数据相应的自增主键。

常用场景 2：

通过先执行 `select UUID()` 获得的随机值赋值给 Javabean 的某个变量后

# mybatis 逆向工程

是一个工具，用于根据指定数据库和表自动生成对应的 Javabean、映射文件以及映射器接口。

## 环境配置

1. 引入依赖

```xml
<dependency>
    <groupId>org.mybatis.generator</groupId>
    <artifactId>mybatis-generator-core</artifactId>
    <version>1.3.7</version>
</dependency>
```

2. 在 resources 根目录下导入 generatorConfig. xml 配置文件，不需要 mybatis. xml 配置文件

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <context id="testTables" targetRuntime="MyBatis3">
        <commentGenerator>
            <!-- 是否去除自动生成的注释 -->
            <property name="suppressAllComments" value="true" />
        </commentGenerator>
        <!--数据库连接的信息：驱动类、连接地址、用户名、密码 -->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/j20_db"
                        userId="root"
                        password="">
            <!--是否只扫描上述指定数据库中的表-->
            <property name="nullCatalogMeansCurrent" value="true"/>
        </jdbcConnection>

        <!-- javaModelGenerator 生成javaBean的配置选项
             targetProject: 生成javaBean类的位置
             targetPackage: 生成javaBean的类名-->
        <javaModelGenerator targetPackage="com.liuyiwei.bean"
                            targetProject="./src/main/java">
            <!-- enableSubPackages:是否允许子包,是否让schema作为包的后缀
                 即targetPackage.schemaName.tableName -->
            <property name="enableSubPackages" value="true" />
            <!-- 从数据库返回的值是否清理前后的空格 -->
            <property name="trimStrings" value="true" />
        </javaModelGenerator>


        <!-- sqlMapGenerator Mapper映射文件的配置信息
            targetProject: mapper映射文件生成的位置
            targetPackage: 生成mapper映射文件放在哪个包下-->
        <sqlMapGenerator targetPackage="com.liuyiwei.mapper"
                         targetProject="./src/main/resources">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="true" />
        </sqlMapGenerator>

        <!--
           javaClientGenerator 生成映射器接口的配置
           targetProject: mapper接口生成的位置
           targetPackage: 生成mapper接口放在哪个包下

           ANNOTATEDMAPPER
           XMLMAPPER
           MIXEDMAPPER
        -->

        <javaClientGenerator type="XMLMAPPER"
                             targetPackage="com.liuyiwei.mapper"
                             targetProject="./src/main/java"><!--/-->
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="true" />
        </javaClientGenerator><!---->
        <!-- 指定数据库表 -->

            <!-- 指定所有数据库表 -->

            <!--<table tableName="%"
                   enableCountByExample="false"
                   enableUpdateByExample="false"
                   enableDeleteByExample="false"
                   enableSelectByExample="false"
                   enableInsert="false"
                   enableDeleteByPrimaryKey="true"
                   enableSelectByPrimaryKey="true"
                   selectByExampleQueryId="false" ></table>-->

               <!-- 指定数据库中要生成的表，要和数据库中对应-->
                <!--byExample 构造where条件 和你的列相关 几乎涵盖了所有你能想到的条件 -->
               <table  tableName="j20_user_t"
                       enableCountByExample="false"
                       enableUpdateByExample="false"
                       enableDeleteByExample="false"
                       enableSelectByExample="false"
                       enableInsert="true"
                       enableDeleteByPrimaryKey="true"
                       enableSelectByPrimaryKey="true"
                       selectByExampleQueryId="false"
                       domainObjectName="User"
               /> <!--User User UserMapper UserMapper.xml-->
                <!--domainObjectName 设定各生成代码文件的前缀 xxxx的Javabean xxxMapper xxxMapper.xml-->
                <table tableName="j20_student_t" domainObjectName="Studentz"/>
                <table tableName="j20_account_t" domainObjectName="Account"/>


        <!--      <table schema="" tableName="orders"></table>
             <table schema="" tableName="items"></table>
             <table schema="" tableName="orderdetail"></table>
      -->
               <!-- 有些表的字段需要指定java类型
                <table schema="" tableName="">
                   <columnOverride column="" javaType="" />
               </table> -->
    </context>
</generatorConfiguration>
```

3. 在 com. liuyiwei 包下导入用于生成代码的 Generator. java 文件：

```java
public class Generator {
    public void generator() throws Exception {
        List<String> warnings = new ArrayList<>();
        boolean overwrite = true; //指向逆向工程配置文件
        //new File 写的相对路径对应的是当前应用的工作路径，而main方法默认的工作路径是 project-directory
        File configFile = new File("src/main/resources/generatorConfig.xml");
        System.out.println(configFile.getAbsolutePath());
        ConfigurationParser cp = new ConfigurationParser(warnings);
        Configuration config = cp.parseConfiguration(configFile);
        DefaultShellCallback callback = new DefaultShellCallback(overwrite);
        MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config, callback, warnings);
        myBatisGenerator.generate(null);
    }

    public static void main(String[] args) throws Exception {
        try {
            Generator generatorSqlMap = new Generator();
            generatorSqlMap.generator();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

需要注意的点：
1. mybatis-generator 是代码和配置文件生成器，按照某个规则生成，不保证映射文件没有问题，比如列名出现关键词，mybatis 逆向工程识别不到，需要自己增加 `order`
2. 逆向工程的映射文件是增量更新，即再次运行 Generator 会在原来的映射文件的基础上再次更新，会造成映射文件中生成相同的内容，包含相同的 id，最终导致报错
3. 如果想要重新生成 → 你就先删除原有的映射文件再重新生成
4. 不要在已有的项目中执行逆向工程 → 新建一个项目单独作为逆向工程的项目 → javabean、mapper 接口和映射文件复制到你已有的项目（targetPackage 中配置包目录和你的项目一致） → 会覆盖你原有的 javabean 或接口，造成你已经完成的代码的丢失

## 自动生成的 xxxByExample 使用方法

xxxbyExample 表示根据条件做数据操作，映射文件中通常包含了 where

如何使用？

new Example (). createCriteria (). and 各种条件

criteria 标准

xxxbyPrimaryKey：根据主键

xxxbySelective 表示根据形参是否为 null 做选择性的操作，包含了 if 标签

如果方法名没有 selective ，则不管是否为 null，都完成赋值操作，即可能往数据库中写入 null。

```java
StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
StudentExample studentzExample = new StudentExample();
ArrayList<String> names = new ArrayList<>();
names.add("aaa");
names.add("bbb");

// 通过Example对象建立过滤标准（Criteria）
studentExample.createCriteria().andIdEqualTo(3).andStudentNameIn(names);
// xxxByExample(传入一个Example对象)
List<Studentz> studentz = mapper.selectByExample(studentExample);
```

## 参考

[mybatis – MyBatis 3 | 简介](https://mybatis.org/mybatis-3/zh/index.html)
