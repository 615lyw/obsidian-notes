# 安装步骤

《尚硅谷资料参考课件：第01章_Linux下MySQL的安装与使用》

# MySQL 8.0 和 5.7 默认字符集的区别

- 8.0 默认 uft8mb4
- 5.7 默认 latin1，不支持中文

安装完 5.7 记得改默认字符集（修改 my.cnf）

# 请求到响应过程字符集的变化

认识到 MySQL 在请求到响应过程中会有多次字符集转换这件事。

- character_set_client 解析客户端发来的字符命令
- character_set_connection
- character_set_results 返回客户端时的结果集编码

# 各级别的字符集（层层继承）

认识到创建数据库、表、字段时默认字符集继承关系。

- 服务器级别 character_set_server
- 数据库级别 character_set_database
- 表级别
- 字段（列）级别

# 常见比较规则含义

- xxx_ci：case insensitive 大小写不敏感
- xxx_cs：case sensitive 大小写敏感

# MySQL 中 utf8 和 utf8mb4 的区别

MySQL 中显示的 uft8 实为 utf8mb3（3 个字节，阉割版 [[UTF-8]]），utf8mb4 为 4 个字节（uft8 most bytes 4）。

4 个字节可以完整的表示 [[Unicode]] 所有字符了。

# 如何避免大小写问题？

可通过 SQL 大小书写规范解决：[[MySQL SQL 书写#SQL 书写规范]]

# 两种 sql_mode 模式

- 宽松模式
- 严格模式

常见的 `ONLY_FULL_GROUP_BY`：严格 `GROUP BY` 语法。

## 参考

- [MySQL数据库教程-P101-字符集](https://www.bilibili.com/video/BV1iq4y1u7vj?p=101)
- 尚硅谷资料参考课件：第01章_Linux下MySQL的安装与使用.PDF