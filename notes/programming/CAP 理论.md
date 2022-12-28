---
date created: 2021-05-12, 07:25:21
date modified: 2022-12-28, 20:32:26
---

# Meta

- alias:
- parent :: [[分布式]]
- siblings :: [[BASE 理论]]
- child ::
- refs:
    - [CAP & BASE理论 | JavaGuide](https://javaguide.cn/distributed-system/theorem&algorithm&protocol/cap&base-theorem.html#cap%E7%90%86%E8%AE%BA)
    - [22 | 想成为架构师，你必须知道CAP理论-极客时间](https://time.geekbang.org/column/article/9302)

---

# CAP 理论

> In a distributed system (a collection of interconnected nodes that share data.), you can only have two out of the following three guarantees across a write/read pair: Consistency, Availability, and Partition Tolerance - one of them must be sacrificed.

在分布式系统中，当涉及读写操作时，只能同时满足一致性、可用性和分区容忍性三个中的两个。

## 一致性（Consistency）

> A read is guaranteed to return the most recent write for a given client.

**对于某个指定的客户端，读操作保证能够返回最新的写操作结果。**

理解：该客户端写操作后（写请求可能发往分布式系统中任意节点），能通过读操作（读请求可能发往分布式系统中任意节点）获取之前写操作的结果。(有点像并发中的可见性保证)

更进一步，**任意客户端写操作后，任意客户端读操作都能获取最新的写操作结果。**

## 可用性（Availability）

> A non-failing node will return a reasonable response within a reasonable amount of time (no error or timeout).

非故障节点在一个合理的时间内返回一个响应，而不是超时 [[3 种接口响应结果]]。

非故障节点指的是节点本身没有宕机、仍可以和客户端通信。

响应的结果不一定是正确的。比如库存此时应该返回 100，但返回了 90。

## 分区容忍性（Partition Tolerance）

> The system will continue to function when network partitions occur.

分区特指网络分区，由于网络问题导致节点间暂时无法共享数据，从整体网络分割成多个子网。

在发生网络分区时，系统仍能继续提供合理的服务（能返回合理的响应）。

# CAP 应用

因为网络是不可靠的, 所以 P 必须要满足, 故只能选择 AP 或 CP 架构.

## CP 架构

出现分区时系统仍然提供分布式服务, 要求分布式下数据一致性, 不要求分布式可用性 (非故障节点返回合理值), 即旧数据节点非故障, 但不可用, 返回错误.

## AP 架构

出现分区时系统仍然提供分布式服务, 不要求分布式下保证一致性 (读能读到最新值), 而保证分布式可用性, 读能读到合理值即可.

## CA 架构

不选择 P 即当系统发生网络分区时, 不提供分布式服务, 可以提供单点服务, 天然保证 CA.

# CAP 细节

## CAP 应用粒度为数据级，而非系统级

> C 与 A 之间的取舍可以在同一系统内以非常细小的粒度反复发生，而每一次的决策可能因为具体的操作，乃至因为牵涉到特定的数据或用户而有所不同。

AP、CP 应用粒度应该是数据级别，而非系统级别。系统中的数据可以按照业务场景和要求进行分类，有的数据需要应用 AP 理论，有的需要 CP 理论，不同类型的数据适用不同的理论。比如涉及交易的数据要求 CP，比如热点、热搜只需 AP 即可。

## CAP 隐含了忽略网络延迟的前提条件

CAP 中的 C 在理论上要求系统一旦发生了写操作，瞬时更新所有的数据副本。考虑到网络延迟可知 C 在现实情况下是不可能实现的。所以对于某些业务要求一致性的场景（比如秒杀），在分布式场景下技术上是无法实现。理论要求 CP, 但实际上 CP 做不到, 不能有 P 只能选 CA, 即只能单点写入. 并不意味着系统无法应用分布式架构，只是说“单个用户余额、单个商品库存”无法做分布式，但系统整体还是可以应用分布式架构. 当出现网络分区时, 只会影响部分用户.

## 分区时要考虑 CP 还是 AP, 没有分区时要考虑如何保证 CA

> A system has consistency if a transaction starts with the system in a consistent state, and ends with the system in a consistent state. In this model, a system can (and does) shift into an inconsistent state during a transaction, but the entire transaction gets rolled back if there is an error during any stage in the process.

注意 ⚠️：**一致性并不是指分布式系统所有节点在任何时刻的数据都是一致的**。考虑分布式事务情况，在事务执行过程中，会出现多个节点数据不一致情况。因为事务在执行过程中，client 是无法读取到未提交的数据的，只有等到事务提交后，client 才能读取到事务写入的数据，而如果事务失败则会进行回滚，client 也不会读取到事务中间写入的数据。
