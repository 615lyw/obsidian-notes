微服务架构下经常需要创建多 module 的聚合工程。本质即 Maven 聚合工程。

步骤：

1. New Project 创建 Maven 工程，作为父 POM 工程
2. 删除新创建工程中的 `src` 文件夹，只保留 `pom.xml`，并添加 `<packaging>pom</packaging>`
3. 添加 `<dependencyManagement>` 标签，统一管理版本号 [[Maven#依赖管理]]
4. 