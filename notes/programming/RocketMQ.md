# RocketMQ

# 同类对比

# 部署模型

# 运维

启动流程：先启动 Namesrv

配置文件修改

启动注册中心

## mqbroker

启动 broker，默认启动在 10911 端口

```bash
./mqbroker -n localhost:9876 autoCreateTopicEnable=true

.......
The broker[MBP, 192.168.31.107:10911] boot success. serializeType=JSON and name server is localhost:9876
```

## mqadmin

作用：管理命令

**查看帮助**

在 mqadmin 下可以查看有哪些命令

```sh mqadmin```

**查看具体命令的使用**

```sh mqadmin help 命令名称```

例如，查看updateTopic 的使用

```sh mqadmin help updateTopic```

```bash
sh mqadmin help updateTopic

usage: mqadmin updateTopic [-b <arg>] [-c <arg>] [-h] [-n <arg>] [-o <arg>] [-p <arg>] [-r <arg>] [-s <arg>]
       -t <arg> [-u <arg>] [-w <arg>]
 -b,--brokerAddr <arg>       create topic to which broker
 -c,--clusterName <arg>      create topic to which cluster
 -h,--help                   Print help
 -n,--namesrvAddr <arg>      Name server address list, eg: 192.168.0.1:9876;192.168.0.2:9876
 -o,--order <arg>            set topic's order(true|false)
 -p,--perm <arg>             set topic's permission(2|4|6), intro[2:W 4:R; 6:RW]
 -r,--readQueueNums <arg>    set read queue nums
 -s,--hasUnitSub <arg>       has unit sub (true|false)
 -t,--topic <arg>            topic name
 -u,--unit <arg>             is unit topic (true|false)
 -w,--writeQueueNums <arg>   set write queue nums
```

例子：

```bash
sh mqadmin updateTopic -b localhost:10911 -n localhost:9876 -t user-register-succ-topic
```

给 localhost:10911 的 broker 配置 user-register-succ-topic 并告知 localhost:9876 的注册中心。
