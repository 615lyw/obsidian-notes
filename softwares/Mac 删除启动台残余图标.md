# Mac 删除启动台残余图标

法一：长按图标至抖动删除

法二：

1. 打开 Finder，前往文件夹 `/private/var/folders`
2. 搜索 `com.apple.dock.launchpad`
3. 进入文件夹，可以看到 `db`
4. 复制 `db` 所在路径
5. 打开终端 `cd` 进入
6. 执行命令：

```bash
$ sqlite3 db "delete from apps where title='Windows10 Applications';"
$ killall Dock
```

注意 title 中字符串空格问题。
