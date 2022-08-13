# JDBC

Java 数据库访问 API，模仿 ODBC（全称：Open Database Connectivity，C 语言数据库访问 API）。

Java API -> 驱动管理器 -> 驱动程序 ->（数据库协议）-> 数据库

使用步骤：
1. 配置 JDBC URL
2. 注册驱动（遵循 JDBC4 的驱动程序 jar 包会自动在 DriverManager 中注册驱动）
3. 获取连接
4. 创建执行平台
5. 执行 SQL
6. 处理结果集
7. 关闭连接
