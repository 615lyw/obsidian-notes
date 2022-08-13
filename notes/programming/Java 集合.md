---
date created: 2021-10-08, 18:02:29
date modified: 2022-05-26, 15:51:49
---

# 集合类层级关系图






[[Class ArrayList]]

[[Class HashMap]]

[[Interface Set]]


LinkedHashSet 简介

- HashSet 子类
- 底层是 HashMap + 双向链表
- 双向链表定义了迭代顺序，不受扩容影响
- 不同步

# TreeSet

概述：

- 底层数据结构：TreeMap
- 元素有序
- 不同步
- 不能存储 null 元素（底层是红黑树）
- 如果在 TreeSet 构造方法中传入 Comparator，则按照 Comparator 比较结果来；场景：TreeSet 中存储别人写好的类
- 如果 TreeSet 构造方法中没有传入 Comparator，则按照对象的自然顺序进行排序，要求 TreeSet 中存储的元素对象必须实现 Comparable 接口）
- 如果同时有 Comparator + Comparable，则按照 Comparator 顺序（看源码）
- 千万不要修改 Set 和 Map 集合里面元素的属性（不会重新调整）
- 不依赖于 hashCode 和 equals 方法

TreeSet 是如何保证元素的唯一性？

TreeMap key 的唯一性（Map 的 put 方法）

## 3 TreeMap

- 底层的数据结构是红黑树
- 如果创建对象时，没有传入 Comparator 对象，**键**将按自然顺序进行排序
- 如果创建对象时，传入了 Comparator 对象，键将按 Comparator 进行排序
- 不允许 null 键，允许 null 值
- 不同步
