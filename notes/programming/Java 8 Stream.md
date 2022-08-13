# 流

流是比集合更高概念的一种数据视图。

操作集合的形式是显式告诉计算机怎么处理集合中每个元素，用到的是最基本的元素（循环、判断）；而流则像 SQL 一样只需说明要完成什么任务，把具体的实现交给流库。流库可以通过单 CPU 执行多线程分段处理再汇总，也可分配给多 CPU 处理。

思维变化：迭代处理集合 -> 通过流处理集合。

# 流 VS 集合

流

# 操作流的典型流程

1. 创建一个流
2. 将初始流转换为其他中间流
3. 执行终止操作，产生结果

# 创建流

- 集合：实现了 Collection 接口的类可以通过 `stream` 和 `parallelStream` 创建流
- 数组：通过 Arrays 类的 `stream` 和 `parallelStream` 创建流
- Stream 类的工厂方法：`empty`、`of`、`concat`、`generate`、`iterate`、`range`、`rangeClosed` 以及 `builder` 等方法创建流

## 无限流与空流

通过 `Stream.empty()` 创建一个空的流（没有元素）。

无限流：拥有无限多元素的一个序列。

如何创建无限流？

- `Stream.generate(Supplier<T> s)`
    - 流中每个元素均通过 s 函数产生
- `Stream.iterate(iterate(T seed, UnaryOperator<T> f))`
    - 流中第一个元素为 seed，第二个元素为 f(seed)，第三个元素为 f(f(seed))，以此类推

无限流的使用场景是什么？

唔知道呀。

# 参考

- https://www.zhangshengrong.com/p/nDa9joWgNj/
- 《Java 核心技术卷二》
