# Reference

- [Jackson JsonNode](http://tutorials.jenkov.com/java-json/jackson-jsonnode.html)

# JSON 类库种类

1. jackson
2. gson
3. fastjson

# 依赖配置

maven 依赖：

```xml
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-databind</artifactId>
  <version>${jackson-version}</version>
</dependency>
```

只用导入 jackson-databind 即可。

![](https://gitee.com/ryan615/PicGoBed/raw/master/image/20201015141032.png)

# JsonNode vs. ObjectNode

- 相同点：两者都代表一个 Json 对象
- 不同点：JsonNode 是**不可变的**（只能从中取值，而不能新增、删除和修改），ObjectNode 是可变的

# Read JsonNode From JSON

```java
String json = "{ \\"f1\\" : \\"v1\\" } ";

ObjectMapper objectMapper = new ObjectMapper();

JsonNode jsonNode = objectMapper.readTree(json);

System.out.println(jsonNode.get("f1").asText());
```

# Write JsonNode to JSON

```java
ObjectMapper objectMapper = new ObjectMapper();

// readJsonIntoJsonNode() produces a JsonNode object
JsonNode jsonNode = readJsonIntoJsonNode();

String json = objectMapper.writeValueAsString(jsonNode);
```

# Get JsonNode Field

```java
JsonNode field1 = jsonNode.get("field1");
JsonNode field2 = jsonNode.get("field2");
```

# Get JsonNode Field At Path

JSON structure:

```json
{
  "identification" :  {
        "name" : "James",
        "ssn: "ABC123552"
    }
}
```

第一个 `/` **必须存在**，代表最外面一层 JsonNode 根节点。

以绝对路径的形式书写。

```java
JsonNode nameNode = jsonNode.at("/identification/name");
```

# Convert JsonNode Field

Filed Value ---> data type

- 123456 ---> 123456
- 123456 ---> "123456"
- 123456 ---> 123456.0

```java
// 假设 f2 的值为：123456
String f2Str = jsonNode.get("f2").asText();
double f2Dbl = jsonNode.get("f2").asDouble();
int    f2Int = jsonNode.get("f2").asInt();
long   f2Lng = jsonNode.get("f2").asLong();
```

# Convert With Default Value

# 两种情况

1. json 字符串中不存在 filed ---> jsonNode is null
2. json 字符串中 filed 值为 null ---> jsonNode is a NullNode

如果 JSON 字符串中有目标对象没有的属性，转换过程会报错。

如何解决：
在目标对象类上加 `@JsonIgnoreProperties(ignoreUnknown=true)`

```java
// To ignore any unknown properties in JSON input without exception:
@JsonIgnoreProperties(ignoreUnknown=true)
```

# 枚举的序列化和反序列化

序列化：

```java
@JsonValue
public int getValue() {
    return code;
}
```

反序列化：

```java
@JsonCreator
public static Size from(int code) {
    for (Size size : values()) {
        if (size.code == code) {
            return size;
        }
    }
    return null;
}
```
