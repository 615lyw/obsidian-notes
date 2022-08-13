scp 通过 [[SSH 协议]] 实现本地文件（或目录）和远程文件（或目录）之间相互复制。

# 下载远程服务器文件或目录到本地

文件：`scp user@ip:/path/to/file local/path`

目录：`scp -r user@ip:/remote/path local/path`

# 上传本地文件或目录到远程服务器

文件：`scp local_file user@ip:/remote/path`

目录：`scp -r local_dir user@ip:/remote/path`

# 参考

[scp 命令](https://www.linuxprobe.com/scp-cmd-usage.html)
