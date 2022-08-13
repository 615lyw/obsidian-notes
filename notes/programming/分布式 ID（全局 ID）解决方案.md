# 分布式 ID（全局 ID）

#分布式 #微服务

## 为什么需要分布式 ID？

单机系统中数据的唯一性通常通过数据库自增 ID 来保证，在分布式环境下，多节点之间的数据唯一性便需要分布式 ID 来保证。

## 分布式 ID 需要满足什么要求？

- 全局唯一
- 高性能
- 高可用
- 方便易用
- 安全
- 可以包含具体业务含义
- 有序递增

## 常见解决方案

- 数据库
    - 数据库表自增 ID
    - 数据库号段模式
    - Redis (NoSQL) `incr` 原子自增
- 算法
    - UUID
        - 缺点：字符串占用空间大，索引效率低；非递增；没有业务含义不可读
    - Snowflake (雪花算法)
- 开源框架
    - 百度 UidGenerator
    - 美团 Leaf
    - 滴滴 Tinyid

## 参考

- [44 | 弹力设计篇之“幂等性设计”-极客时间](https://time.geekbang.org/column/article/4050)
- [分布式 ID | JavaGuide](https://javaguide.cn/distributed-system/distributed-id.html#分布式-id-介绍)