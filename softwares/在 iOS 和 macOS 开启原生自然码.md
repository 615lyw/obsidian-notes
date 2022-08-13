# iOS 开启原生自然码

为了更改一个 key-value 键值对，要下载 Xcode，太重了。

使用 `·plist` 属性文件编辑器即可，或通过命令行 `plutil` 进行更改。

恢复设备失败就再试一下。

# 在 macOS 上使用原生的自然码

```
defaults write com.apple.inputmethod.CoreChineseEngineFramework shuangpinLayout 5
```

## 参考

[iOS 原生自然码 - 少数派](https://sspai.com/post/60751)
