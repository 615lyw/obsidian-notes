微服务架构下经常需要创建多 module 的聚合工程。本质即 Maven 聚合工程。

步骤：

1. New Project 创建 Maven 工程，作为父 POM 工程
2. 删除刚创建的 Module（IDEA 单 Module 即 Project）中的 `src` 文件夹，只保留 `pom.xml`，并添加 `<packaging>pom</packaging>`
3. 在父 Module 上右键创建子 Module

![[CleanShot 2022-09-06 at 21.48.58@2x.png]]

创建完成后，Project 视图如下：

![[CleanShot 2022-09-06 at 21.51.38@2x.png]]

父 POM 中自动引入子 Module：

![[CleanShot 2022-09-06 at 21.52.17@2x.png]]

子 POM 中自动继承父 POM：

![[CleanShot 2022-09-06 at 21.53.02@2x.png]]