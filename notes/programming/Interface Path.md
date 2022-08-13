# Interface Path

JDK 7 引入 `java.nio.file.Path` 包。

Path 只是一个接口，有不同的文件系统实现：Windows、macOS 和类 Unix，在什么平台上开发便使用对应的文件系统实现，故使用结果与平台相关。

Path 类重要有用的特性：

- 按下标提取文件或目录名
- 按下标范围获取子路径
- 路径拼接
- 相对路径计算

“路径拼接”有个很有用的特性：能支持 Windows 路径拼接上 macOS 和类 Unix 路径！

例如：在 Windows 平台有一个 Path 类，代表 `E:\foo\bar`，拼接 `dir1/dir2`，前者是 Windows 路径，后者是类 Unix 路径，调用 `path.resolve("dir1/dir2")` 的结果是 `E:\foo\bar\dir1\dir2`。 ^c5c4ca
