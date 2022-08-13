定义：

- 不包含重复元素
- 有些 set 不允许有 null，有的允许但最多包含一个 null 元素
- set 集合可以有序，也可以无序

常用子类：

- HashSet
- LinkedHashSet
- TreeSet

# HashSet

- 底层：哈希表 HashMap
- 无序（迭代顺序不一定等于添加顺序）
- 允许存在最多一个 null
- 不同步集合

## HashSet 为什么使用哈希表？

HashSet 集合的目的就是为了存储不可重复的元素，模拟数学上集合的概念。其实可以使用任何数据结构（本质上都是数组或链表）来完成该任务，每当添加一个新元素时，依次比较集合中每个元素是否相等，因为需要多次调用 equals 方法，故效率太低。

使用哈希表根据 hashCode 完成存储位置映射，使得只需要和链表上的元素进行比较即可。

## HashSet 如何检查元素是否重复

[[equals 和 hashCode 实现规范]]

### 实现原理

HashSet 底层基于 HashMap，在 HashMap 提供的 API 基础上进行包装。

```java
private transient HashMap<E,Object> map;

// Dummy value to associate with an Object in the backing Map
private static final Object PRESENT = new Object();

public boolean add(E e) {
    return map.put(e, PRESENT)==null;
}
```

**问题实质为：HashMap 如何保证 Key 的不可重复？**

HashMap 的 put () 方法原理：当添加新元素时，通过新元素的 hashCode 计算出映射到 Table 表的物理位置，如果当前位置没有其他元素，则直接添加；如果有其他元素，先依次比较链表中其他元素的 hashCode 值：

- 若 hashCode 不同，则为不同的元素
- 若 hashCode 相同，可能为相同或不相同的元素，使用 equals 比较，若为 true，则不允许添加重复元素
