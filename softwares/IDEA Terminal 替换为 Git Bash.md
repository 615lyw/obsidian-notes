# IDEA Terminal 替换为 Git Bash

主要用于 Windows，Git Bash 相比 CMD 通过简单配置可以很好的支持中文显示且还支持 Linux 命令。

`settings`–>`Tools`–>`Terminal`–>`Shell path: C:\Program Files\Git\bin\bash.exe`

解决在命令行查看 git commit 中文乱码问题：

在`C:\Program Files\Git\etc\bash.bashrc`末尾行追加如下内容：`export LANG="zh_CN.UTF-8"`

关闭当前 Terminal 连接重启。
