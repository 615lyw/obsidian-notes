# Mybatis 源码包分类

包分类：

- 基础功能包：提供如文件读取功能、反射操作等基础功能
- 配置解析包：在系统初始化阶段运行（[[Mybatis 运行总览#初始化阶段]]），负责配置的解析与存储
- 核心功能包：在数据库操作阶段运行（[[Mybatis 运行总览#数据库读写阶段]]），各包配合完成数据库操作

![](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/ZdVZjR.png)

阅读顺序：基础功能包 -> 配置解析包 -> 核心功能包。

## 基础功能包

[[Mybatis exceptions 包]]

[[Mybatis refection 包]]

## 配置解析包

## 核心功能包