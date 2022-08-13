Meta:

- Parents: [[Java IO]]
- Status: #New 
- Refs: 

---

File 类旨在提供一种与操作系统无关的**文件或目录的抽象路径表示**。

例如：

- 文件：`/usr/local/a.txt`
- 目录：`/usr/local/bin`

# getAbsolutePath() VS getPath()

getPath 返回的是构建 file 实例时 `File(String pathname)` 传入的字符串 pathname。

getAbsolutePath 返回值由两部分构成：`resolve(System.getProperty("user.dir"), f.getPath())`。

补充：`user.dir` 系统属性在本地 IDEA 项目中值为当前项目路径。
