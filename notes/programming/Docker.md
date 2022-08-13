# Docker

- Go 编写的开源项目
- 一种容器 [[虚拟化技术]]

## 核心思想

- 把（代码+运行环境）打包到一块，保证各环境一致
- 使用更轻量的容器虚拟化技术，大大减少系统资源的占用

## docker 解决了什么问题？

- 环境部署更简单
    - 以前开发提供 jar 包和相关数据库脚本与文档，运维部署基础环境，搭建各环境麻烦，使用 Docker 可以实现多个基础环境打包以及代码与环境的关系适配好
- 更轻量：更快的启动时间+更高效利用系统资源
- 方便、快速扩缩容：依据上面两个优势

## docker 架构

![](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/Rn8Erz.png)

C/S 架构，Registry 为中央镜像仓库，Daemond 服务端负责管理镜像和容器。

# docker 三大对象

## 镜像 image

镜像是静态的，包含运行时所需的程序，库，配置等。

特点：

- 镜像内容在构建后不会改变（只读），故不会存储运行时产生的数据
- 镜像是分层构建的，在后一层上的改动只发生在自己这一层，而不会真正改动前一层

## 容器 container

容器是镜像运行时的实体。

特点：

- 容器是一个独立的空间，拥有自己的网络配置、文件配置
- 容器可以创建、删除、启动、停止、暂停（具有生命周期）
- 容器 = 镜像层 + 容器临时存储层，存储层随着容器的删除而消失，故需要永久保存的数据需外挂存储

## 仓库

仓库用于存储、分发镜像。

一个 **Docker Registry** 中可以包含多个**仓库**（ `Repository` ），每个仓库可以包含多个**标签**（ `Tag` ），每个标签对应一个镜像。

最常使用的 Registry 公开服务是官方的 [Docker Hub](https://hub.docker.com)，这也是默认的 Registry。也可以搭建私有的 Registry 服务。

如何确定软件版本？
通过 `仓库名:标签`，例如 ubuntu: 16.04，仓库表示存储什么软件，标签对应软件的版本。如果忽略标签，则为 `ubuntu:latest`。

从官方镜像仓 Docker Hub 拉取镜像缓慢，需 [配置 Docker 国内镜像加速器](https://yeasy.gitbook.io/docker_practice/install/mirror)

# 镜像使用

- 获取镜像 `docker pull ubuntu:18.04`
- 列出所有顶层镜像：`docker image ls`
- 删除镜像：`docker image rm`

虚悬镜像概念：虚悬镜像是仓库名和标签名均为 `<none>` 的顶层镜像，例如：

```bash
$ docker image ls
REPOSITORY           TAG                 IMAGE ID            CREATED             SIZE
redis                latest              5f515359c7f8        5 days ago          183 MB
nginx                latest              05a60462f8ba        5 days ago          181 MB
mongo                3.2                 fe9198c04d62        5 days ago          342 MB
<none>               <none>              00285df0df87        5 days ago          342 MB
ubuntu               18.04               329ed837d508        3 days ago          63.3MB
ubuntu               bionic              329ed837d508        3 days ago          63.3MB
```

虚悬镜像的产生原因在于同一个软件同一版本进行维护更新，新旧版本不能同名，故旧版本成为虚悬镜像，可通过 `docker image prune` 命令删除。

## 通过 Dockerfile 定制镜像

`docker commit` 用于在原镜像基础上持久化容器存储层，形成新的镜像。（有缺点，仅用于被入侵后保存现场）

Dockerfile 中每一条指令会构建一层镜像，每一条指令内容应该描述该层如何构建。

## 参考

