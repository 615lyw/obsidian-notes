# Maven

- Apache 下纯  Java 开发的开源项目
- 用于 Java 平台的项目构建，依赖管理，项目信息管理等

## 什么是构建 (build)？

构建 = 清理 -> 编译 -> 运行单测 -> 生成测试报告 -> 打包 -> 部署。

## 为什么需要 Maven？

Maven 抽象了一个完整的, 标准的构建生命周期模型. 我们可以简单地通过一条命令让构建过程全自动化。

[[Maven 安装&预配置]]

# maven 核心概念

## POM

When in the presence of Maven folks, speaking of a project is speaking in the philosophical sense, beyond a mere collection of files containing code. A project contains configuration files, as well as the developers involved and the roles they play, the defect tracking system, the organization and licenses, the URL of where the project lives, the project's dependencies, and all of the other little pieces that come into play to give code life. It is a one-stop-shop for all things concerning the project. In fact, in the Maven world, a project does not need to contain any code at all, merely a `pom.xml`.

POM："Project Object Model"。依据面向对象思想理解，把项目当做一个对象，也符合 maven 是纯 Java 开发特点。

项目不仅仅是代码文件的集合，包含了例如：

- 配置文件
- 开发人员
- 项目描述
- 所属组织
- 依赖项
- 版本
- 许可证
- 等等......

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
 
  <!-- The Basics -->
  <groupId>...</groupId>
  <artifactId>...</artifactId>
  <version>...</version>
  <packaging>...</packaging>
  <dependencies>...</dependencies>
  <parent>...</parent>
  <dependencyManagement>...</dependencyManagement>
  <modules>...</modules>
  <properties>...</properties>
 
  <!-- Build Settings -->
  <build>...</build>
  <reporting>...</reporting>
 
  <!-- More Project Information -->
  <name>...</name>
  <description>...</description>
  <url>...</url>
  <inceptionYear>...</inceptionYear>
  <licenses>...</licenses>
  <organization>...</organization>
  <developers>...</developers>
  <contributors>...</contributors>
 
  <!-- Environment Settings -->
  <issueManagement>...</issueManagement>
  <ciManagement>...</ciManagement>
  <mailingLists>...</mailingLists>
  <scm>...</scm>
  <prerequisites>...</prerequisites>
  <repositories>...</repositories>
  <pluginRepositories>...</pluginRepositories>
  <distributionManagement>...</distributionManagement>
  <profiles>...</profiles>
</project>
```

## maven 坐标

maven 坐标是 pom. xml 中最小必填项。

- groupId：公司域名倒置，如 com. alibaba
- artifactId：项目名称，如 dubbo、druid
- version：项目版本

## 打包方式

`<packaging>...</packaging>`

可选值：

- pom
- jar（默认值）
- war
- 等等

## “约定优于配置”原则（目录结构）

>  假如工具的约定与你的期待相符，你便可省去相关配置；如果不相符，你可以通过配置来达到所期待的方式。

如果项目的目录结构不符合约定，执行 maven 相关指令也不会报错。

目录结构约定如下：

![](https://gitee.com/ryan615/PicGoBed/raw/master/image/20200908144147.png)

## 本地仓库和中央仓库

# 项目构建

## 属性

maven 属性可以理解为**变量的定义与引用**.

- 内置属性
- POM 属性
- 自定义属性
- Settings 属性
- Java 系统属性
- 环境变量属性

## 多环境

此处结合 SpringBoot 多环境搭配使用.

`application.yml` :

```yml
spring:
  profiles:
    active: dev
```

为了解决 `application.yml` 中 dev 写死的情况, 由于 SpringBoot 项目同时也是 maven 结构, 故可以通过 maven 的属性引用来解决.

`pom.xml` 中定义多环境:

```xml
<profiles>
	<profile>
  	<id>dev</id>
    <properties>
      <!-- 自定义标签 -->
    	<package.env>dev</package.env>
    </properties>
    <activation>
    	<activeByDefault>true</activeByDefault>
    </activation>
  </profile>
  
  <profile>
  	<id>test</id>
    <properties>
    	<package.env>test</package.env>
    </properties>
</profiles>
```

在 `application.yml` 中通过属性引用自动调整 profile:

```yaml
spring:
  profiles:
    active: @package.env@
```

注意 `yml` 文件引用和 `properties` 文件引用方式不同, `properties` 为 `${package.env}` .

## 资源过滤

maven 属性引用默认只在 pom. xml 中才会被解析. 故 `src/main/resources/` 目录下的配置文件如果也想使用 maven 属性引用则需要开启资源过滤.

资源过滤所需插件: `maven-resources-plugin` , 其默认行为只是把主资源文件和测试资源文件**复制**到各自对应的编译输出目录下, 通过配置可以开启资源过滤, 即用定义值替换 maven 属性引用.

最后通过命令行激活指定的 profile.

在项目的根目录下执行相关 maven 指令。前提：根目录下存在 pom. xml 项目配置文件。

1. 清理：`mvn clean`
删掉 target 编译目录。

2. 编译：`mvn compile`
在符合约定的目录结构下执行该指令会生成 target 目录。

3. 测试：`mvn test`

maven 会自动运行所有的单元测试，而不需要手动一个个点击。

4. 打包：`mvn package`
- 编译、测试均成功后会在 target 目录下生成 jar 包（默认打 jar 包）
- 过程：编译 → 测试 → 打包

5. 将 `mvn package` 生成的 jar 包安装至本地仓库：`mvn install`

# 依赖管理

依赖分两种：

- 直接依赖
- 传递性依赖

a 依赖于 b，若只安装 a 仍不能运行，还需安装 b。依赖管理让你只需要关注所需的直接依赖（a），Maven 会自动安装好间接依赖（b）。（类似 Unix 上的包管理器）

## 如何理解 scope？

主代码
- 编译：编译 classpath
- 运行：运行 classpath

测试代码：测试 classpath
- 编译
- 运行

scope:

- compile：编译 classpath + 测试 classpath + 运行 classpath
- test：测试 classpath
- provided：编译 classpath + 测试 classpath
- runtime：测试 classpath + 运行 classpath

依赖调解：

第一原则：路径最近者优先

第二原则：路径相同，第一声明者优先

## 什么是依赖冲突？

项目依赖中存在同一依赖的不同版本。

# 仓库污染

![](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/maven仓库污染对比.jpg)

# 如何通过 Maven 和 SpringBoot 配置多环境？

- 在 pom. xml 中通过 profile 标签定义多环境
- 配置资源过滤所需插件 maven-resources-plugin
- 替换 application. yml 中的属性引用

# 多模块 Maven 项目

## 聚合与继承

聚合是为了通过命令一次性构建多个模块。

继承是为了提取重复项，统一管理。

# 参考

《Maven 实战》