---
date created: 2022-05-18, 22:59:19
date modified: 2022-06-05, 10:04:30
---

# Meta

- parent :: [[MySQL SQL 书写]]
- siblings ::
- child ::
- refs:

---

# 非分组列为什么不能单独出现在 SELECT 检索列表中？

因为非分组列的值可能不一样，最终变成一行时不知道如何选取。

# 如何实现非分组列出现在 SELECT 检索列表中？

- 把非分组列放到聚集函数中
- 调整 sql_mode 模式，取消 `ONLY_FULL_GROUP_BY` 限制

# 分组列存在 NULL 时会怎样？

NULL 也会作为一个独立的分组存在。

# 嵌套分组时，聚集函数（比如 `COUNT(*)`）统计规则？

针对小组进行统计。

例如下面这个 SQL，统计以 major 最小分组后的行数。

![CleanShot2022-05-05at12.28.56@2x](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/CleanShot%202022-05-05%20at%2012.28.56@2x.png)

```
mysql> SELECT department, major, COUNT(*) FROM student_info GROUP BY department, major;
+-----------------+--------------------------+----------+
| department      | major                    | COUNT(*) |
+-----------------+--------------------------+----------+
| 航天学院        | 电子信息                 |        1 |
| 航天学院        | 飞行器设计               |        1 |
| 计算机学院      | 计算机科学与工程         |        2 |
| 计算机学院      | 软件工程                 |        2 |
+-----------------+--------------------------+----------+
4 rows in set (0.00 sec)
```

# 嵌套分组概念

1. GROUP BY 后面接多列，用逗号分隔即有多个分组列
2. 先按第一列分组，在按第二列分组

**`WHERE` 子句和 `HAVING` 子句的区别？**

`WHERE` 在分组前进行过滤，`HAVING` 在分组后进行过滤。

**GROUP_CONCAT 函数**

把每个分组中指定列的值聚集起来形成一个字符串：

- 使用前提：分组
- 默认使用逗号分隔
- 列转行
- GROUP_CONCAT 接受任意列

```sql
SELECT team_id, GROUP_CONCAT(player_name)
FROM player
GROUP BY team_id;
```
