# Mybatis-Plus

  [官网](https://baomidou.com/)

  只对 Mybatis 做增强，不做改变，不影响 Mybatis 的原生功能。

  特性：

  - 单表强大的 CRUD
  - 支持 Lambda 形式调用
  - 支持主键自动生成
  - 支持 ActiveRecord 模式
  - 内置代码生成器
  - 内置分页插件
  - 内置性能分析插件

# SpringBoot 下安装配置

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>mybatis-plus-latest-version</version>
</dependency>
```

引入 `MyBatis-Plus` 之后请不要再次引入 `MyBatis` 等依赖，因为 `MyBatis-Plus` 已经通过传递性依赖引入了 `MyBatis`。

# SSM 传统编程模式

- 在 Dao 层接口中写抽象方法
- XML 或注解中写 SQL(**MP优势：对于简单 CRUD 可以省略不写**)
- Service 中调用 Dao 层接口
- Controller 中调用 Service 接口
- **MP 默认策略**
  - 插入时主键生成策略
    - 不用在代码中指定实体类主键值，MP 默认采用雪花算法生成(**不是自增主键**)
  - 当插入或更新一个实体对象时，如果实体的某属性值为 null，则表对应的列不会出现在插入或更新的 SQL 语句中。
    - 更新操作即不改变原有值
    - 插入操作即对应值为 null
  - 实体类名与表名、实体属性与列名默认转换策略
    - 例如 user 表 → User 实体类
    - event_id 列名 → eventId
- 常用注解
  - 原理：将 Java 实体类的类名和属性名按照默认映射关系转换为 SQL 语句执行
  - 建立非默认映射关系
    - @TableName("表名")+实体类：实体类名 → 表名
    - @TableId+实体主键属性：主键属性名 → 列名
    - @TableField("列名")+实体非主键属性：非主键属性名 → 列名
- **通用 Mapper**
  - `public interface UserMapper extends BaseMapper`
  - CRUD
    - **条件构造器 Wrapper**
      - 查询：QueryWrapper
        - `new QueryWrapper()`
        - 主动调用 `.or()` 表示紧接着^^下一个方法^^不是用 `AND` 连接!(不调用 `.or()` 则默认为使用 `AND` 连接)
        - 两参：`("列名", val)`，例如 `queryWrapper.eq("name", "zhangsan")` 生成的预编译 SQL 为：`WHERE name = "zhangsan"`
        - 模糊查询
          - `.like("列名", val)` → `LIKE %val%`
          - `.likeRight("列名", val)` → `LIKE val%`
      - 更新：
    - 查询
      - Doc: https://baomidou.com/guide/crud-interface.html#select
      - 根据 ID 批量查询
        - `List selectBatchIds(Collection idList);`
        - 生成的预编译 SQL：`WHERE id in (?, ?, ?)`
      - 根据条件 Map 查询
        - `List selectByMap(Map columnMap);`
        - key 为表中列名，而不是实体类中属性名
        - 生成的预编译 SQL：`WHERE key1 = ? AND key2 = ?`
    - 插入
      - `sql // 插入一条记录 int insert(T entity);`
      - 排除非表字段的 3 种方式
        - 场景：实体类中有一些字段(临时字段，不存储)在数据表中不存在，需要忽略
        - 方案一：声明为 `transient` 不参与序列化
        - 方案二：声明为 `static`
          - 缺点：如果每个对象都需要该字段，则该方法不适用
        - 方案三：@TableField(exist=false)

# 支持枚举

- 场景：实体类的成员变量可以直接使用枚举类
- 版本要求：

## 1、创建枚举类并声明待存属性

通过 `@EnumValue` 标识需要数据库存入的字段。

```java
@Getter
public enum GradeEnum {

    PRIMARY(1, "小学"),
    SECONDORY(2, "中学"),
    HIGH(3, "高中");

    GradeEnum(int code, String descp) {
        this.code = code;
        this.descp = descp;
    }

    @EnumValue
    private final int code;
    private final String descp;
}
```

## 2、配置枚举类扫描路径

```yaml
mybatis-plus:
    # 支持统配符 * 或者 ; 分割
    typeEnumsPackage: com.baomidou.springboot.entity.enums
```
