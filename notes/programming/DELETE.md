---
date created: 2022-05-18, 22:41:06
date modified: 2022-06-05, 10:05:11
---

# Meta

- parent :: [[MySQL SQL 书写]]
- siblings ::
- child ::
- refs: [MySQL 5.7 Reference Manual :: 13.2.2 DELETE Statement#Multi-Table Deletes](https://dev.mysql.com/doc/refman/5.7/en/delete.html)

---

# 最简单形式

`delete from table where status = 2;`

# 多表删除

`FROM` 子句中指定别名，`DELETE` 和 `FROM` 之间使用别名。

删除 t1 表和 t2 表中的记录，其 id 在两张表中均存在。

```sql
DELETE a1, a2 
FROM t1 AS a1 INNER JOIN t2 AS a2
WHERE a1.id=a2.id;
```
