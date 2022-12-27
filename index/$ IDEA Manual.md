[[配置 IDEA 自动生成 serialVersionUID]]

# 版本控制

## 文件对比

在Git tool window 的 Log 标签中可以看到 commit 历史记录，点击任一 commit，右侧显示 Changed Files 面板。

**该面板显示本次 commit 相对于父 commit 的变更文件，双击任一文件，弹出 Difference Viewer，右侧展示本 commit 的文件内容，左侧是父 commit 的文件内容。（注意左右顺序）**

## 查看 merge 节点是如何解决冲突的

点击 merge 节点，在Git tool window-Changed Files 面板，会显示3部分内容：

- 哪些文件发生了冲突，双击文件后可以看到冲突是如何解决的
- merge 节点相比两个父节点，各自变更的文件

# 调试

[官方文档-调试](https://www.jetbrains.com/help/idea/2021.1/using-breakpoints.html#set-breakpoints)

## 断点

断点类型：

- 行断点：通常说的断点，程序停在指定代码行
- 方法断点：进入或退出某方法时（或抽象方法实现之一）暂停程序
- 成员变量观察断点：当成员变量被修改或读取时暂停程序
- 异常断点：当全局抛出异常时暂停程序

前3种是直接在源码上通过打标记设置的，3种断点在 Gutter 中的展示图标也有区别。

还可以在3种非异常断点上进行属性配置，指定断点条件。

## 方法断点

在抽象方法上断点，可以自动跳转到运行时的具体实现方法。

## 配置断点属性

遍历一个大List时，想停在某个满足特定条件时：
在断点行右键断点，填写condition。

![](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/zSmEe5.png)


[[IDEA Terminal 替换为 Git Bash]]

# Maven 依赖管理

https://www.jetbrains.com/help/idea/maven-support.html

# EmacsIdea 插件

常用：

1. 复制一个 word 到光标处：ctrl+w，ctrl+w
2. 删除一个 word：ctrl+d，ctrl+w

![[Pasted image 20221130145533.png]]