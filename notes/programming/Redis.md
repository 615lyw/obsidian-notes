#  Redis

> a more complex version of memcached

用 C 语言高效实现了多种数据结构的缓存服务器。

# 安装

[[Redis 安装]]

# Redis 核心配置文件 redis.conf

```conf
*1. Redis 默认以前台应用方式运行，而不是守护进程，可以通过该配置项修改，使⽤yes启动守护进程
daemonize no

2. 当客户端闲置多⻓长时间后关闭连接(单位是秒)
timeout 300 

*3. 指定Redis监听端⼝，默认端⼝为6379（作者在⼀⽚博⽂中解释了为什么选⽤6379作为默认端⼝，因为6379在⼿机按键上MERZ对应的号码，⽽MERZ取⾃意⼤利歌⼿Alessia Merz的名字）
port 6379 

*4. 绑定的主机地址
bind 127.0.0.1(只有本机才能访问Redis)
#bind 0.0.0.0(所有的IP都允许访问Redis)

5. 指定⽇志记录级别，Redis共⽀持四个级别：debug、verbose、notice、warning
loglevel notice

6. 数据库数量(单机环境下)，默认存储数据库id为0，可以使⽤select <dbid> 命令在连接上指定数据库id（不像MySQL可以让我们任意创建数据库）
databases 16

// 非常重要（触发什么条件就将内存中的数据写回至磁盘）
// 条件：同时达到时间和次数的要求
// 当 Redis 异常结束时，可能发生数据丢失
*7. RDB 持久化策略：指定在多⻓时间内，有多少次更新操作，就将数据同步到数据⽂件，可以多个条件配合 save <seconds> <changes>，Redis 默认配置⽂件提供了三个条件
save 900 1 
save 300 10 
save 60 10000

8. 持久化⽂件名 
dbfilename dump.rdb 

*9. 指定快照存储⾄本地数据库时是否压缩，默认为yes，Redis采⽤LZF（压缩算法）压缩，如果为了节省CPU时间，可以关闭该选项，但会导致数据库⽂件变得巨⼤（效果几十GB → 几KB）
rdbcompression yes

*10. 设置Redis连接密码，如果配置了连接密码，客户端在连接Redis的时候需要通过 AUTH <password> 命令提供密码，默认关闭
# requirepass foobared 

// AOF 配置
*11. 指定是否在每次操作后进⾏⽇志记录，Redis在默认情况下是关闭的 appendonly no

*12. AOF⽂件的名字
appendfilename "appendonly.aof"

*13. AOF 策略,分为三种，always表示每次写操作都会记录⽇志，everysec表示每秒记录⼀次⽇志，no表示不记录⽇志，默认为 everysec
# appendfsync always 
appendfsync everysec 
# appendfsync no
```

## 持久化策略小结

- RDB：快照，对 Redis 内存中的数据直接备份
- AOF：日志型，每次对数据进行更新的操作，Redis都会记录在内存中，根据配置的aof 策略来决定是否写回、以什么形式写回

快照发生条件：

- Redis 服务器正常关闭（ctrl+c）
- 满足 RDB 持久化条件

共同点：Redis 异常挂掉时，都有可能丢失数据，除了 `appendfsync always`，但此配置会导致每次写操作都会进行一次磁盘I/O，和 MySQL 的读写效率都一致了。

# Redis常用数据类型及其应用场景

官方支持5种数据类型：String、List、Hash（二维表）、Set（无序集合）、SortSet（有序集合）。

## 基本操作

```bash
# 获取当前所有的key
keys *

# 清空所有数据
flushall
```

## String

## List

## Hash

## Set

## SortSet

# Jedis 客户端的使用

不能存储对象

# RedisTemplate 客户端的使用

sb-1.x 底层使用的是 jedis

sb-2.x 底层使用的是 lettuce

能存储对象，优于 Jedis。

## 1 导包

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

## 2 配置

sb 其实在 RedisAutoConfiguration 中已经帮我们自动注册了 redisTemplate 组件：

```java
public class RedisAutoConfiguration {
    @Bean
    public RedisTemplate redisTemplate(RedisConnectionFactory redisConnectionFactory) {
        RedisTemplate redisTemplate = new RedisTemplate();
        redisTemplate.setConnectionFactory(redisConnectionFactory);
        StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();
        redisTemplate.setKeySerializer(stringRedisSerializer);

        // 到此 value 已不再乱码
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        
        // 以下设置的是反序列化时能够重新获得java对象
        ObjectMapper objectMapper = new ObjectMapper();
        // 对于不是基本类型的变量显示全类名
        objectMapper.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        // 设置值的属性可⻅
        objectMapper.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        jackson2JsonRedisSerializer.setObjectMapper(objectMapper);
        redisTemplate.setValueSerializer(jackson2JsonRedisSerializer);

        return redisTemplate;
    }
}
```

注意 `@ConditionalOnMissingBean(name = "redisTemplate")` 当容器中没有一个叫 redisTemplate 的 bean，即没有手动配置一个 bean 的名字叫 redisTemplate 时，就会使用 sb 自动提供的这个，故想要通过手动配置覆盖 sb 自动提供的，注册类中的方法名必须为 redisTemplate（方法名即为 bean id，即组件名）。

### 提供 java 对象

因为该对象要通过网络传输给Redis，故需要实现序列化，java 中一个类能够序列化需要 implements Serializable 接口，Serializable 只是一个语义接口，没有要求实现任何方法，但必须有一个 serialVersionUID（通过IDEA自动生成）。

```java
@Data
public class Person implements Serializable {

    private static final long serialVersionUID = 7192339941062356305L;

    private String height;
    private String weight;
}
```

### 使用自己的配置类

```java
@Configuration
public class RedisConfig {
    @Bean
    public RedisTemplate redisTemplate(RedisConnectionFactory redisConnectionFactory) {
        RedisTemplate redisTemplate = new RedisTemplate();
        redisTemplate.setConnectionFactory(redisConnectionFactory);
        StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();
        // 法一：设置key和value的序列化器，否则存入key-value时会乱码
        /*redisTemplate.setKeySerializer(stringRedisSerializer);
        redisTemplate.setValueSerializer(stringRedisSerializer);*/
        
        // 法二：使用jackson来实现value的序列化，为了能让value存储java对象
        redisTemplate.setKeySerializer(stringRedisSerializer);
        
        
        return redisTemplate;
    }
}
```

### 使用自定义的 Redis 连接配置

默认提供的也是下述内容，即如果使用的是默认配置则可以不用写

```properties
spring.redis.host=localhost
spring.redis.port=6379
```

## 3 使用

此处通过继承 DemoApplicationTests，运行 @Test 的方法时触发 RedisTest 类加载，先加载父类，父类有 @SpringBootTest 注解，即这个测试类运行在一个 Spring 容器环境中。

```java
public class RedisTest extends DemoApplicationTests {
    @Autowired
    private RedisTemplate redisTemplate;

    @Test
    public void test1() {
        redisTemplate.opsForValue().set("name", "liuyiwei");
    }
}
```

@SpringBootTest 的作用：会读取我们自己写的 @Configuration 的配置类以此生成一个 Spring 容器环境。

```java
@SpringBootTest
class DemoApplicationTests {

    @Test
    void contextLoads() {
    }
}
```

key没有序列化的乱码结果：

```bash
127.0.0.1:6379> keys *
1) "\xac\xed\x00\x05t\x00\x04name"
```

# Redisson 客户端的使用

# Redis 内存淘汰策略

在 redis.conf 文件中可设置 Redis 最大可使用内存，最大值为 512G。当内存达到 maxmemory 时，会根据 maxmemory-policy 选择淘汰部分 key-value，类似内存的页面置换算法。

```conf
maxmemory 512G
```

**Redis 5.0 以后提供了 8 种内存淘汰策略（新增2个有关 lfu 的策略）：**

- allkeys：在整个数据集中进行挑选

- volatile（易变的）：可以理解为状态会改变的数据，设置了过期时间的数据集（设置了过期时间，到了一定的时间后状态会改变）

由 maxmemory-policy 进行配置：

```conf
# The default is:
# maxmemory-policy noeviction
```

- volatile-lru：在设置了过期时间的数据集中使用LRU算法选择淘汰 key
- volatile-lfu
- volatile-ttl
- volatile-random
- allkeys-lru
- allkeys-lfu
- allkeys-ttl
- allkeys-random
- noeviction（驱逐）

```conf
############################## MEMORY MANAGEMENT ################################

# Set a memory usage limit to the specified amount of bytes.
# When the memory limit is reached Redis will try to remove keys
# according to the eviction policy selected (see maxmemory-policy).
#
# If Redis can't remove keys according to the policy, or if the policy is
# set to 'noeviction', Redis will start to reply with errors to commands
# that would use more memory, like SET, LPUSH, and so on, and will continue
# to reply to read-only commands like GET.
#
# This option is usually useful when using Redis as an LRU or LFU cache, or to
# set a hard memory limit for an instance (using the 'noeviction' policy).

# MAXMEMORY POLICY: how Redis will select what to remove when maxmemory
# is reached. You can select one from the following behaviors:
#
# volatile-lru -> Evict using approximated LRU, only keys with an expire set.
# allkeys-lru -> Evict any key using approximated LRU.
# volatile-lfu -> Evict using approximated LFU, only keys with an expire set.
# allkeys-lfu -> Evict any key using approximated LFU.
# volatile-random -> Remove a random key having an expire set.
# allkeys-random -> Remove a random key, any key.
# volatile-ttl -> Remove the key with the nearest expire time (minor TTL)
# noeviction -> Don't evict anything, just return an error on write operations.
#
# LRU means Least Recently Used
# LFU means Least Frequently Used
#
# Both LRU, LFU and volatile-ttl are implemented using approximated
# randomized algorithms.
#
# Note: with any of the above policies, Redis will return an error on write
#       operations, when there are no suitable keys for eviction.
#
#       At the date of writing these commands are: set setnx setex append
#       incr decr rpush lpush rpushx lpushx linsert lset rpoplpush sadd
#       sinter sinterstore sunion sunionstore sdiff sdiffstore zadd zincrby
#       zunionstore zinterstore hset hsetnx hmset hincrby incrby decrby
#       getset mset msetnx exec sort
```

# zset 底层数据结构

详见数据结构笔记 - **跳表**。

# 参考

[【尚硅谷】Redis 6 入门到精通 超详细 教程_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Rv41177Af)
