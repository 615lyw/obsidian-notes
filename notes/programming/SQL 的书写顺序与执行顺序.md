---
date created: 2022-06-14, 17:03:24
date modified: 2022-08-09, 18:15:00
---

# Meta

- alias:
- parent :: [[MySQL SQL 书写]]
- siblings ::
- child ::
- refs:

---

# SQL 的书写顺序与执行顺序

1. 关键字书写顺序

```sql
SELECT ... FROM ... WHERE ... GROUP BY ... HAVING ... ORDER BY ... LIMIT
```

2. 子句的执行顺序

```sql
FROM > WHERE > GROUP BY > HAVING > SELECT > DISTINCT > ORDER BY > LIMIT
```
