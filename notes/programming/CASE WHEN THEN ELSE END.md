---
date created: 2022-05-18, 22:55:14
date modified: 2022-05-24, 19:58:46
---

# Meta

- parent:: [[MySQL SQL 书写]]
- siblings::
- child::
- refs:
    - [MySQL CASE Function](https://www.w3schools.com/sql/func_mysql_case.asp)
    - [MySQL :: MySQL 5.7 Reference Manual :: 13.6.5.1 CASE Statement](https://dev.mysql.com/doc/refman/5.7/en/case.html)
---

语法：

```sql
# 写法一
CASE
    WHEN condition THEN statement
    [WHEN condition THEN statement] ...
    [ELSE statement]
END [AS alias]


# 写法二
CASE case_value
    WHEN when_value THEN statement
    [WHEN when_value THEN statement] ...
    [ELSE statement]
END [AS alias]
```

- 返回第一个 condition 为 true 的结果
- 如果 when 的 condition 均不为 true，返回 else 结果
- 如果 when 的 condition 均不为 true，没有 else 部分，返回 NULL
