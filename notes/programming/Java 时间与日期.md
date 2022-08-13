---
date created: 2022-02-07, 15:07:00
date modified: 2022-02-24, 14:03:09
---
背景：[[时间计量系统]]

# 旧时间日期类

`java.util` 包

## Date

表示日期和时间（绝对时间）。

缺点：

- 无时区信息，故不能转换时区进行输出，总是以当前计算机系统的默认时区输出 `Sun Sep 05 09:53:54 CST 2021`
- 不能对时间、日期进行计算

## Calendar

Calendar 相比 Date 多了时间日期计算功能和通过 TimeZone 进行时区转换的功能。

是一个抽象类（不能实例化），是所有日历类的模板。

只能通过 `Calendar.getInstance()` 获取实例，且为当前时间。

## SimpleDateFormat

用于格式化 Date

线程不安全

可以设置时区调整打印格式

## TimeZone

通过时区 ID 字符串来获取时区对象：`TimeZone tzGMT9 = TimeZone.getTimeZone("GMT+09:00");`

# 新时间日期类

[[Java 8 新时间日期 API]]
