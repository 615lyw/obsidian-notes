# Mac 屏蔽系统更新并去除小红点

如果没有第一时间关闭软件更新导致后台下载软件更新，先通过如下命令屏蔽软件更新：

```bash
sudo softwareupdate --ignore "macOS Monterey"
defaults write com.apple.systempreferences AttentionPrefBundleIDs 0
killall Dock
```

`--ignore` 可忽略选项：

- macOS Monterey
- macOS Big Sur
- macOS Catalina
- macOS Mojave Security Update 2021-004（根据具体编号填写）

重置：`sudo softwareupdate --reset-ignored`
