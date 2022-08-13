---
date created: 2022-02-24, 14:03:09
date modified: 2022-08-09, 19:41:36
---

# ArrayList 扩容机制

```java
	/**
     * 默认初始容量大小
     */
    private static final int DEFAULT_CAPACITY = 10;

	/**
     * 使用有参构造器，用户手动指定初始容量为0时指定的数组对象
     */
	private static final Object[] EMPTY_ELEMENTDATA = {};
    
	/**
     * 使用无参构造器时指定的数组对象
     */
    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};

	/**
     * 底层实现
     */
	Object[] elementData;

    /**
     *默认构造函数，使用初始容量10构造一个空列表(无参数构造)
     */
    public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }
    
    /**
     * 带初始容量参数的构造函数。（用户自己指定容量）
     */
    public ArrayList(int initialCapacity) {
        if (initialCapacity > 0) {
            //初始容量大于0：创建initialCapacity大小的数组
            this.elementData = new Object[initialCapacity];
        } else if (initialCapacity == 0) {
            //初始容量等于0：指定空数组
            this.elementData = EMPTY_ELEMENTDATA;
        } else {
            //初始容量小于0，抛出异常
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        }
    }
```

**以无参数构造方法创建 ArrayList 时，实际上初始化赋值的是一个空数组。当真正对数组进行添加元素操作时，才真正分配容量。即向数组中添加第一个元素时，数组容量扩为 10。**

`add()` 中 `ensureCapacityInternal(size + 1)` 每次传入比当前元素个数多 1 的值作为最小所需容量。

```java
	/**
     * 将指定的元素追加到此列表的末尾。 
     */
    public boolean add(E e) {
        ensureCapacityInternal(size + 1);
        elementData[size++] = e;
        return true;
    }

	// 判断用户是通过什么方式创建的数组列表
    private void ensureCapacityInternal(int minCapacity) {
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
            // 此处只进入一次！
            // 获取默认的容量和传入参数的较大值
            minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity);
        }

        // modCount++
        ensureExplicitCapacity(minCapacity);
    }

	// 判断是否需要扩容
    private void ensureExplicitCapacity(int minCapacity) {
        modCount++;

        // 装满了，需要扩容
        if (minCapacity - elementData.length > 0)
            //调用grow方法进行扩容，调用此方法代表已经开始扩容了
            grow(minCapacity);
    }

	/**
     * 要分配的最大数组大小
     */
    private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;

    /**
     * ArrayList扩容的核心方法。
     */
    private void grow(int minCapacity) {
        int oldCapacity = elementData.length;
        // 将oldCapacity 右移一位，其效果相当于oldCapacity / 2
        // 将新容量更新为旧容量的1.5倍
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        // 检查新容量是否大于最小需要容量，若还是小于最小需要容量，那么就把最小需要容量当作数组的新容量
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
       // 如果新容量大于 MAX_ARRAY_SIZE, 执行 hugeCapacity() 方法来比较 minCapacity 和 MAX_ARRAY_SIZE
       // 如果minCapacity大于最大容量，则新容量则为`Integer.MAX_VALUE`，否则，新容量大小则为 MAX_ARRAY_SIZE 即为 `Integer.MAX_VALUE - 8`。
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
```

`DEFAULTCAPACITY_EMPTY_ELEMENTDATA` 的内存地址和 `EMPTY_ELEMENTDATA` 不一样。

如果用户是通过指定初始化容量大小（假定为 0）来创建 ArrayList：

容量变化：0 → 1 → 2 → 3 → 4 → 6 → 9 → 13 → 19 → ……

如果用户使用无参构造器来创建 ArrayList：

容量变化：0 → 10 → 15 → 22 → ……

文字简化描述扩容机制：

1. 根据用户是否指定初始化容量：
    - 如果用户是通过有参构造器手动指定初始化容量大小来创建数组列表，则按照手动指定的初始容量值的 1.5 倍（**除以 2，再加 1 倍**）进行扩容
    - 如果用户使用无参构造器，当第一次执行 `add` 方法时进行扩容，直接扩容到 10，之后在 10 的基础上进行 1.5 倍扩容
2. 什么时候执行 `grow` 扩容函数：当数组填满时
