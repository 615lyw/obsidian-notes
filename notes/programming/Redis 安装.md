# Redis 安装

## 手动编译安装

去 [Redis 官网](https://redis.io/) 下载 Redis，下载下来的是源码，需要自己手动编译后才能使用。

下载、解压缩后，进入解压缩后的 Redis 目录：

```bash
# 执行编译命令
% make
```

启动服务端：

```bash
% cd src
# 运行服务端
% ./redis-server
```

新开一个会话连接，使用 Redis 客户端连接上服务端：

```bash
% cd src
# 运行客户端
% ./redis-cli
```

## 通过 homebrew 安装

安装： `brew install redis`

启、停、重启：

`brew services start mysql`

`brew services stop mysql`

`brew services restart mysql`