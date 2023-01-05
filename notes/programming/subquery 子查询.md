Meta:

- parents: [[MySQL SQL 书写]]
- refs:

---

# 关联子查询和非关联子查询的区别？

关联子查询需要用到外部查询中的值，非关联子查询不需要用到外部查询中的值，可独立运行。

# 子查询分类

标量子查询：子查询结果为一个值。

列子查询：子查询结果不是一个值，而是一列（多个值，相当于一个列表）。
搭配使用 `IN` 或 `NOT IN` 关键字。

行子查询：子查询结果是一行（有多个列）。
行子查询等值比较写法：

```SQL
SELECT * 
FROM student_score 
WHERE (number, subject) = (SELECT number, '母猪的产后护理' FROM student_info LIMIT 1);
```

表子查询：子查询的结果为多行多列。
表子查询等值比较写法：

```SQL
SELECT * 
FROM student_score 
WHERE (number, subject) 
IN (SELECT number, '母猪的产后护理' FROM student_info WHERE major = '计算机科学与工程');
```

# 子查询执行流程

- 非关联子查询执行顺序由内而外
- 关联子查询执行顺序类似嵌套 for 循环

# 子查询能否出现在 SELECT 检索列表中？

可以，同常见于 WHERE 中的子查询一样使用，也可分为关联子查询和非关联子查询。

例如：

```sql
SELECT team_name, (SELECT count(*) FROM player WHERE player.team_id = team.team_id) AS player_num 
FROM team;
```

上述子查询属于关联子查询。根据 team 中的每一行的 team_id 去 player 表进行子查询。

# 通过子查询把把 Empty set 转成 NULL 显示

- SELECT (Empty set) 返回 NULL
- 通过 IFNULL 函数

# 边界条件

考虑这个语句执行结果：`SELECT s1 FROM t1 WHERE s1 IN (SELECT s1 FROM t2);`。

```sql
create table t1 (id int primary key auto_increment, s1 int);
insert into t1 (s1) values (10);

create table t2 (id int primary key auto_increment, s1 int);
insert into t2 (s1) values (0),(NULL),(1);
```

结果是 Empty set。

因为 t2 表存在一个 NULL 值，无法进行大小判断，故 10 > ALL 不成立。

具体推理过程需要进行语句转换：

- `IN (subquery)` 等价于 `= ANY (subquery)` 或 `= SOME (subquery)`
- `NOT IN (subquery)` 等价于 `<> ALL (subquery)`
- `= ANY (subquery)` 需要和每一个值进行比较，再通过 OR 连接起来
- `<> ALL (subquery)` 也需要和每一个值进行比较，再通过 AND 连接起来
- 最后再结合 [[MySQL AND 和 OR 操作符]] 理解

# EXISTS 和 NOT EXISTS

`WHERE EXISTS (SELECT *)` 把列放在子查询中，比较子查询结果是否为空集，决定 WHERE 条件是否为真。

# SQL IN 和 EXISTS 语句如何互相改写？

- SELECT * FROM A WHERE cc IN (SELECT cc FROM B)
- SELECT * FROM A WHERE EXIST (SELECT cc FROM B WHERE B. cc=A. cc)
