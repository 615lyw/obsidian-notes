# 安装

[官网](https://maven.apache.org/download.cgi)下载，解压缩，添加 bin 目录至 PATH。

执行 `mvn -v` 显示：

```bash
Apache Maven 3.5.3 (3383c37e1f9e9b3bc3df5050c29c8aff9f295297; 2018-02-25T03:49:05+08:00)
Maven home: /Users/ryan/backend/maven/apache-maven-3.5.3
Java version: 1.8.0_261, vendor: Oracle Corporation
Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_261.jdk/Contents/Home/jre
Default locale: zh_CN, platform encoding: UTF-8
OS name: "mac os x", version: "10.14.6", arch: "x86_64", family: "mac"
```

# 设置国内镜像

设置国内镜像，在 `<mirrors>` 标签下：

```xml
<mirror>
  <id>nexus-aliyun</id>
  <mirrorOf>central</mirrorOf>
  <name>Nexus aliyun</name>
  <url>http://maven.aliyun.com/nexus/content/groups/public</url>
</mirror>
```

# 设置本地仓库路径

将主机所有项目所依赖的 jar 包均放置在一个特定的目录（仓库）里。jar 包不会因为在多个项目中存在而冗余。
