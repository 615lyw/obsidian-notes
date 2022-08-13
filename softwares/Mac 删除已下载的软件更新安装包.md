# Mac 删除已下载的软件更新安装包

1. 重启电脑，同时按下 cmd+R，看到白苹果的进度条时就可以松开手，然后会进入恢复模式
2. 进入终端，输入 `csrutil disable` 关闭系统完整性保护（SIP）
3. 终端中输入 `reboot` 重启电脑
4. 删除升级文件 `sudo rm -rf /Library/Updates`
5. 重新进入恢复模式，打开系统完整性保护 `csrutil enable`
