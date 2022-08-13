---
date created: 2022-01-25, 19:02:29
date modified: 2022-06-05, 10:04:07
---

# Meta

- parent :: [[MOC MySQL]]
- siblings ::
- child ::
- refs:

---

# 列转行、行转列

列转行
行转列

[例题](https://leetcode.cn/problems/rearrange-products-table/)

1. 把每列拆分成一个 `SELECT` 语句，列名作为 value，原先的 value 作为新列
2. 再通过 `UNION ALL` 合并结果

# UNION

```sql
SELECT expression1, expression2, ... expression_n
FROM tables
[WHERE conditions]
UNION [ALL | DISTINCT]
SELECT expression1, expression2, ... expression_n
FROM tables
[WHERE conditions];
```

`UNION` 默认为 `UNION DISTINCT`，会对结果集进行去重（如果是多列，进行组合去重。同 [[DISTINCT]] 用法）

`UNION ALL` 只是简单的把两个结果集拼接起来。

注意上下两个 `SELECT` 中的检索列数量要一致。

# 过滤

- `WHERE`
- `HAVING`

日期判等问题 `WHERE order_date = '2021-05-10'`
当 order_date 是 `datetime` 类型时会出问题

因为 MySQL 会将 order_date 自动转换为 `datetime` 类型：'2021-05-10 00 :00: 00'，故 '2021-05-10 19 :57: 06' 这条记录会被过滤掉。

解决方案：`WHERE DATE(order_date) = '2021-05-10'`

别名、聚合函数引用问题：

- 在 WHERE 中使用 SELECT 中定义的别名时会报错，因为 WHERE 先于 SELECT 执行，此时别名还未创建
- 解决方案：利用 FROM 执行顺序先于 WHERE，将含有别名列的查询放入子查询中，这样就能在外层 WHERE 中使用别名列

[[SQL 的书写顺序与执行顺序]]

[[如何查看、创建和删除 MySQL 索引]]
