---
date created: 2022-07-04, 20:43:56
date modified: 2022-07-04, 20:43:56
---

# Meta

- alias:
- parent :: [[Java 集合]]
- siblings ::
- child ::
- refs: [Java 8系列之重新认识HashMap - 美团技术团队](https://tech.meituan.com/2016/06/24/java-hashmap.html)

---

HashMap 实现中要点：

- 为什么叫哈希桶数组？
    - 数组的每一个位置都是一个“桶”，意味着可以装很多元素
- **数组大小总是 2 的幂次方**
    - 即使在构造方法中给定任意值的容量大小，也会通过 `tableSizeFor()` 转换为 2 的幂次方
    - 为什么？
        - 目的是通过与运算提高哈希效率
        - 当 length 为 2 的幂次方时：`hash % length = hash & (length - 1)` 成立
- 如何计算一个 key 对应的数组下标？
    - 1 哈希计算：调用 `key.hashCode()` 计算 hash 值（[[equals 和 hashCode 实现规范]]）
    - 2 取模计算：对上步 hash 计算 `(n - 1) & hash` 获取数组下标
- **JDK 1.8 之前存储结构为数组 + 链表（通过拉链法解决哈希冲突），1.8 后为数组+链表+红黑树。（链表达到一定长度（先要数组达到一定长度）会转换为 [[红黑树]]）**
- `put()` 流程
    - 计算 key 对象对应的数组下标
    - 若对应下标位置还没有元素，则直接插入
    - 若对应下标位置有元素，判断是否是相同 key 对象（**判断 key 是否为相同对象涉及 [[equals 和 hashCode 实现规范]]**）
        - 若是相同对象则直接覆盖
        - 若不是相同对象，则去链表或树中查询，若无则插入，若有则覆盖（若链表长度达到一定值进行树化）
        - 怎么判断 key 是否为相同对象？（滴滴面试题）
            - `hash 是否相等 && (key 内存地址是否相等 || key equals 是否相等)`
            - 不同的 hash 值一定对应不同的 key 对象, 可加速判断
            - 但若 hash 值相等, 可能是不同的 key, 还需进一步判断
            - 即 hash 用于加速 hash 值不相等时的判断速度，用内存地址和 equals 方法来兜底判断保证正确性
- 什么时候进行 resize 扩容？几倍扩容？
    - 当 HashMap 中元素个数大于阈值 threshold 时
    - 两倍扩容
- `resize()` 流程（**扩容流程**）
    - JDK 1.7
        - 遍历原数组中每个元素（顺序遍历数组，顺序遍历链表），重新计算下标，放到新数组中去
        - 采用了头插法，若元素在新旧数组中下标仍一致，迁移后元素顺序倒序
    - JDK 1.8
        - **优化点：旧数组中同一位置的元素，rehash 后在新数组下标要么和原下标一样，要么是原数组下标 + 原数组大小**
        - 实现：判断高位是 0 还是 1
- 成员变量含义
    - loadFactor：控制 HashMap 存放元素的疏密程度，可以大于 1
        - 如果内存空间很多而又对时间效率要求很高，可以减小 loadFactor 的值
        - 如果内存空间紧张而对时间效率要求不高，可以增大 loadFactor 的值
    - **capacity：数组长度，即 length（length 更容易辨析）**
    - threshold = length * loadFactor
        - threshold 表示 HashMap 所能容纳的最大元素个数，size > threshold 时进行 resize 扩容