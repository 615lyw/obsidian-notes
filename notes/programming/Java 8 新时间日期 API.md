`java.time` 包：

- 默认按 ISO 8601 标准格式化和解析
- 不变类（线程安全）

# LocalDate、LocalTime、LocalDateTime

本地时间和日期。

# ZonedDateTime

带时区的时间和日期。

ZonedDateTime 源码中的成员变量：

```java
/**  
 * The local date-time. 
 */
private final LocalDateTime dateTime;  
/**  
 * The offset from UTC/Greenwich. 
 */
private final ZoneOffset offset;  
/**  
 * The time-zone. 
 */
private final ZoneId zone;
```

故 **ZonedDateTime = LocalDateTime + ZoneOffset（与 ZoneId 一一映射）**

```java
// 2021-09-05T10:32:16.443+08:00[Asia/Shanghai]
System.out.println(ZonedDateTime.now(ZoneId.of("Asia/Shanghai")));
```

打印形式：北京当前本地时间+时区偏移【时区】

时区转换：

withZoneSameInstant(ZoneId zone)，ZonedDateTime（保持不变） = LocalDateTime（变成 ZoneId 指定的本地时间）  + ZoneOffset（随着变化）

# Instant

时刻

# Duration

表示两个时刻之间的时间间隔

# Period

两个日期之间的天数

# ZoneId，ZoneOffset

时区。

ZoneId 与 ZoneOffset 一一映射。

ZoneId.of(String zoneId)

其中 zoneId 为有限值。进入 ZoneId 源码可以看到支持哪些值。

# DateTimeFormatter

时间日期格式化。

- 不变对象
- 线程安全的，只需创建一次，处处引用

```java
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
```
