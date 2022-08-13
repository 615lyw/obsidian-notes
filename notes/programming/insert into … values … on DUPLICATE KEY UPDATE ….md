---
date created: 2022-05-18, 21:14:59
date modified: 2022-05-21, 09:48:18
---

# Meta

- parent:: [[MySQL SQL 书写]]
- siblings::
- child::
- refs:

---
“插入或更新”：在插入之前先判断 KEY 是否重复，若重复则更新；若不重复则直接插入。

KEY：

- `UNIQUE` index
- `PRIMARY KEY`

案例：

```sql
INSERT INTO t1 (a,b,c) VALUES (1,2,3) ON DUPLICATE KEY UPDATE c=c+1;

               ↑索引检查列      ↑可能冲突字段列                   ↑更新行为
```

对应 (a, b, c) 插入一条新纪录 (1, 2, 3)，其中 a 为主键且表中已经存在 a = 1 的记录，故插入的新纪录不满足主键约束，发生更新行为: c= c + 1，将原记录的 c 改为 c+1。

1. 了解哪些是主键和唯一索引列
2. VALUES 对应的新插入记录中是否存在不满足主键或唯一索引约束的字段或复合字段，若有则触发更新行为；若无则插入记录
