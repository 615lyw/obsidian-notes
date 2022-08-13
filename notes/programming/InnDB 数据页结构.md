---
date created: 2022-06-14, 14:51:30
date modified: 2022-06-14, 15:29:18
---

# Meta

- alias:
- parent :: [[InnoDB 存储引擎]]
- siblings ::
- child ::
- refs: [MySQL 是怎样运行的：从根儿上理解 MySQL - 小孩子4919 - 掘金课程](https://juejin.cn/book/6844733769996304392/section/6844733770046636046)

---

# 数据页结构

为了提高读写效率，以页为单位进行磁盘和内存的交互，一页有多条记录，页大小一般为 16 KB（**16 KB 在物理存储上是连续的**）。

数据页是页的一种类型，存放了表中记录。

![CleanShot2022-06-14at14.39.27](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/CleanShot%202022-06-14%20at%2014.39.27.png)

## 插入新记录，空间分配过程？

1. 插入一条记录时，在 Free Space 部分分配空间划分至 User Records
2. 当 Free Space 用完则需要申请新页

![CleanShot2022-06-14at14.44.09](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/CleanShot%202022-06-14%20at%2014.44.09.png)

## 数据页之间以及数据页内部格式？

File Header 中的前驱和后继指针，使得各个数据页组成一个双向链表，而每个数据页中的记录**会按照主键值从小到大的顺序**组成一个单向链表，每个数据页都会为存储的记录生成一个页目录，在**通过主键查找某条记录时**可以在页目录中使用二分法快速定位到对应的槽（一个槽对应一个记录分组），然后再顺序遍历该槽对应分组中的记录即可快速找到指定的记录。
