---
date created: 2022-08-07, 16:29:31
date modified: 2022-08-07, 16:29:40
---

# Meta

- alias:
- parent ::
- siblings ::
- child ::
- refs:

---

Kafka 由 Scala 语言写的，运行在 JVM 上。

# topic-partition-message

![CleanShot2022-08-07at14.53.00](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/CleanShot%202022-08-07%20at%2014.53.00.png)

- topic
    - topic 是逻辑概念，代表一类业务消息
    - 一个 topic 下面可以有多个 partition

- partition
    - partition 的引入只是为了提升系统性能
    - partition 是有序消息序列
    - 只能往 partition 中追加写消息

message

![CleanShot2022-08-07at14.55.27](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/CleanShot%202022-08-07%20at%2014.55.27.png)

消息以二进制数组形式表示，节省空间

消息在 partition 中的唯一标识是偏移量 offset

如何唯一确定一条消息的三元组？

(topic, partition 编号, 消息在分区中的 offset)

# replica

备份 partition，提升容灾能力。同一分区的不同副本保存的是相同消息。

- 分为 leader replica 和 follower replica
- 生产者和消费者只与 leader 进行通信（leader 负责读写请求），follower 仅用来与 leader 进行同步，充当 leader 候补
- 副本分布在不同 broker 中。若 leader 挂了，从剩余的 follower 中选取一个新 leader 继续提供服务

副本因子：即一个分区创建 N 个 replica。

假设为 3，即创建 3 个副本，从 3 个副本从选举一个作为 leader，剩下两个则为 follower。

如何实现防止宕机导致的数据丢失？

Kafka 保证同一个 partition 的多个 replica 一定不会分配在同一台 broker 上。 当 leader 所在 broker 故障时，可以从其他 broker follower 中选举新 leader。

![CleanShot2022-08-07at14.44.09](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/CleanShot%202022-08-07%20at%2014.44.09.png)

![CleanShot2022-08-07at14.44.40](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/CleanShot%202022-08-07%20at%2014.44.40.png)

# ISR

in-sync replica。

# 客户端开发/使用

类似于使用 HTTP 客户端发送请求的过程。

1. 配置生产者参数，创建生产者对象
2. 构造待发送消息
3. 通过生产者对象发送消息
4. 关闭生产者对象

![CleanShot2022-08-07at17.02.29](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/CleanShot%202022-08-07%20at%2017.02.29.png)

消息对象结构：topic 和 value 必填，其他选填。

![CleanShot2022-08-07at17.01.22](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/CleanShot%202022-08-07%20at%2017.01.22.png)

若 partition 不为空，直接发往对应的分区；若 partition 为空，key 不为空，则通过分区器计算 key 对应的分区；若 key 为空，则轮询发往分区。即若 key 相同，则消息对应的分区也相同。

发送消息有三种模式：

- 发后即忘
- 同步
- 异步

两种异常

序列化

生产者需要用序列化器把对象转换成字节数组发送给 broker，消息者需要用反序列化器把消息从字节数组还原成对象。