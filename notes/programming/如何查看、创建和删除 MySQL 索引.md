---
date created: 2021-11-22, 14:22:40
date modified: 2022-06-14, 17:05:38
---

# Meta

- alias:
- parent :: [[MySQL SQL 书写]]
- siblings ::
- child ::
- refs: [MySQL 是怎样运行的：从根儿上理解 MySQL - 小孩子4919 - 掘金课程](https://juejin.cn/book/6844733769996304392/section/6844733770046668814)

---

`InnoDB` 会自动为主键或者声明为 `UNIQUE` 的列自动建立 `B+` 树索引。

1 建表时创建索引：

```mysql
CREATE TALBE 表名 (
    各种列的信息 ··· , 
    [KEY|INDEX] 索引名 (需要被索引的单个列或多个列)
)
```

其中的 `KEY` 和 `INDEX` 是同义词，任意选用一个就可以。

2 修改表结构的时候添加索引：

```mysql
ALTER TABLE 表名 
ADD [INDEX|KEY] 索引名 (需要被索引的单个列或多个列);
```

3 修改表结构的时候删除索引：

```mysql
ALTER TABLE 表名 
DROP [INDEX|KEY] 索引名;
```

4 查看索引：

```mysql
SHOW INDEX FROM 表名;
```

例子：

```mysql
CREATE TABLE index_demo(
    c1 INT,
    c2 INT,
    c3 CHAR(1),
    PRIMARY KEY(c1),
    INDEX idx_c2_c3 (c2, c3)
);
```

创建的索引名是 `idx_c2_c3`，名称可以随便起，但建议以 `idx_` 为前缀，后边跟着需要建立索引的列名，多个列名之间用下划线 `_` 分隔开。
