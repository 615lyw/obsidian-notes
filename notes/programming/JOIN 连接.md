---
date created: 2022-05-18 22:53:27
date modified: 2022-05-26, 10:54:48
---

# Meta

- parent:: [[MySQL SQL 书写]]
- siblings::
- child::
- refs: [MySQL 是怎样使用的：连接查询 - 小孩子4919](https://juejin.cn/book/6844733802426662926/section/6844733802623795214)

---

# 为什么要使用连接？

在表设计阶段进行了数据拆分，如果要在一次查询中查询多张表的信息，表与表通过“关系”可进行关联。

# 内外连接

驱动表：用这个表的记录去查询另一个表。
被驱动表：被驱动表查的表。

内连接与外连接的根本区别：驱动表中的记录在被驱动表中找不到匹配的行时是否需要加入结果集。
**补充：如果驱动表中的记录为 NULL 时，内连接不会保留该记录至结果集，外连接则会。**

```sql
select employees.*
from employees
inner join dept_emp de on employees.emp_no = de.emp_no;

10001,1953-09-02,Georgi,Facello,M,1986-06-26
10002,1964-06-02,Bezalel,Simmel,F,1985-11-21
10003,1959-12-03,Parto,Bamford,M,1986-08-28

select employees.*
from employees
         left join dept_emp de on employees.emp_no = de.emp_no;

10001,1953-09-02,Georgi,Facello,M,1986-06-26
10002,1964-06-02,Bezalel,Simmel,F,1985-11-21
10003,1959-12-03,Parto,Bamford,M,1986-08-28
<null>,1954-05-01,Chirstian,Koblick,M,1986-12-01

```

内连接：驱动表中的记录在被驱动表中找不到匹配的行，则不加入结果集中。

内连接语法：`SELECT * FROM t1 [INNER | CROSS] JOIN t2 [ON 连接条件] [WHERE 普通过滤条件];` 或 `SELECT * FROM t1, t2;`

推荐写 `INNER JOIN`，含义明确，容易区分。

内连接中 `ON` 和 `WHERE` 等价，不强制要求写。

---

外连接：驱动表中的记录即使在被驱动表中找不到匹配的行，也加入结果集中，被驱动表对应行设置为 `NULL`。

外连接可分为：

- 左外连接：左侧表为驱动表
- 右外连接：右侧表为驱动表

语法要求：使用左连接或右连接时，必须使用 ON 指出连接条件。如图：

![MySQL 官方文档](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/CleanShot%202022-04-23%20at%2017.17.13@2x.png)

# WHERE 和 ON 的区别？

主要是语义上的区别，where 用来限制结果集，on 用来指明连接条件。

# 自连接

表自身进行笛卡尔积，一种特殊的内连接。

自连接可以替换这样的子查询：内外查询都是针对同一张表。

例如：查询和“小王”同一专业的有哪些学生？

如何理解：

1. 搜索条件先指定姓名=小王，可查到小王的基本信息，其中有专业信息 `t1.major`
2. `t1.major(软件工程)=t2.major` 去自身副本 t2 表中查询指定专业的学生

自连接必须使用表别名，因为不知道要使用左表还是右表中的列，两边表列名一样但内容不一样。

![CleanShot2022-04-23at09.32.51@2x](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/CleanShot%202022-04-23%20at%2009.32.51@2x.png)

# 多表连接

```sql
select e.last_name, e.first_name, dp.dept_name
from employees e
         left join (dept_emp de inner join departments dp)
                   on e.emp_no = de.emp_no and de.dept_no = dp.dept_no;

select e.emp_no, e.last_name, e.first_name, de.dept_no
from employees e
         left join dept_emp de on e.emp_no = de.emp_no
         left join departments d on de.dept_no = d.dept_no;
```

# 自然连接

