---
date created: 2022-07-03, 16:02:28
date modified: 2023-04-02, 00:09:02
---

# Meta

- alias:
- parent ::
- siblings ::
- child ::
- refs:
    - [Git Book](https://git-scm.com/book/zh/v2)
    - [廖雪峰 Git 教程](https://www.liaoxuefeng.com/wiki/896043488029600)

---

# Git VS Other VCS

- 其他版本控制系统特点：
    - 基于差异的版本控制
    - 在本地只有最新文件副本，其他信息比如版本历史变更及其文件都保存在远程服务器上
    - 必须连接到远程服务器上才能保存修改
- Git 特点：
    - 分布式
    - 完整存储了所有文件的历史版本
    - 可以离线保存修改

# Git 原理

- Git 保存的是一系列不同时刻的 **快照**
- 当你提交 commit 时，Git 对当前所有文件：
    - 已修改的文件：创建一个快照并保存其索引
    - 其他未修改的文件：只保留一个链接指向之前存储的文件

## 版本库、工作区、暂存区

- 版本库 repository：commits 集合
- 工作区
- 暂存区

## Git 中的对象

> - "commit" objects refer to "tree" objects **representing the snapshot of a directory tree** at a particular point in the history, and refer to "parent" commits to show how they're connected into the project history.
> - "tree" objects represent the state of **a single directory**, associating directory names to "blob" objects containing file data and "tree" objects containing subdirectory information.
> - "blob" objects contain **file data** without any other structure.

![](https://gitee.com/ryan615/PicGoBed/raw/master/image/20201007174345.png)

- commit 对象
	- 指向 tree 对象的指针
	- 作者及邮箱
	- 提交信息
- tree 对象
	- 每个子目录对应一个 tree 对象
	- 指向 blob 对象的指针
- blob 对象
	- 每个文件一个快照对应一个 blob 对象

## HEAD 与 branch

分支本质是一个指针，指向最新 commit：master → f30ab。

HEAD 也是一个指针，可以指向：

- 某个分支：HEAD → master
- 某个 commit（分离头指针 detached HEAD）：HEAD → 34ac2
![](https://gitee.com/ryan615/PicGoBed/raw/master/image/20201011172637.png)

## . git 目录结构

仿 Linux，加粗为目录，不加粗为文本文件。

- HEAD：指向当前分支
- config：git 配置文件（git config 命令配置的内容）
- description：仓库的描述信息
- **hooks**
- **objects**：用哈希值作为目录存放 git 的所有对象
- **refs**：存放分支和 tags 信息
    - **heads**
        - master：存储 master 分支最新 commit_id
        - dev：存储 dev 分支最新的 commit_id
    - **remotes**
        - **origin**
    - **tags**

## git config

`git config` 命令配置的即 . git/config 文件中的值.

### 配置作用域

- System： `/etc/gitconfig` . 系统级别有效.
- Global：home 目录下的 `~/.gitconfig` 文件. 用户级别有效.
- Local：代码库目录的 `.git/config` 文件. 代码库级别有效.

`git config --system -l`，`git config --global -l`，`git config --local -l` 命令分别列出三个作用域下的配置.

Git 在执行命令中会首先查看 local 配置，如果没有找到所需配置会再查看 global 配置，最后再查看 system 配置。

使用 `git config --system`，`git config --global`，`git config --local` 三种不同的选项来修改不同作用域的配置。

### 别名设置

通过命令:

```bash
git config --global alias.st "status"
git config --global alias.cm "commit"
```

通过配置文件:

```txt
[alias]
    st = status
    cm = commit
```

使用举例:

`git st = git status`

## 分离头指针

通常情况下 HEAD 指针总是指向分支指针，而分支指针总是指向该分支最近一次提交，故 HEAD 指向当前分支最近一次提交。如图：

![](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/D9gJuo.png)

HEAD 可以移动，一旦不指向分支名而指向某个 commit，即分离头指针。

```bash
git checkout commit_id
```

## Git 引用

- 本质：指针（KV）。
- 形式：用文件保存 commit_id，文件名便是引用 refs。（文件 - 指针名，文件内容 - 指针值）

### 本地引用

Git 的引用保存在 `.git/refs` 目录下。

```bash
$ find .git/refs
.git/refs
.git/refs/heads
.git/refs/tags
```

`.git/refs/heads` 文件夹保存的是本地分支的引用。其下有一个 master 文件，里面保存的便是 master 分支最近提交的 commit 的 SHA-1 值，故分支的本质即引用。`git branch <branch>` 便是在 `.git/refs/heads` 文件夹下创建一个名为 `<branch>` 的文件。

### HEAD 引用

符号引用，指向其他引用的引用。在分离头指针的情况下保存的是 SHA-1 值。

```bash
$ cat .git/HEAD
ref: refs/heads/master
```

### 远程引用

- 保存在 `refs/remotes` 目录下
- `refs/remotes/origin/master` 文件即表示 origin 远程仓库的 master 分支对应的 SHA-1 值
- 远程引用和本地分支的主要区别是远程引用是 **只读** 的，作为 Git 记录远程服务器上各分支最后已知位置状态
- `git fetch` 会更新 `refs/remotes/origin` 目录下的各文件保存的 SHA-1 值

## Git 引用规范

`<refspec>`

- 建立 **远程引用和本地引用** 或 **远程引用与本地跟踪引用** 之间的 **映射关系**
- 语法：`[+]<src>:<dst>`

# 本地 Git 使用常见场景

## 查看提交历史

`git log --graph --oneline --all`

`git log -3` : 查看最近 3 条 commit 记录.

## 比较文件差异

`git diff` ：显示 **当前工作区** 所有文件的差异。

`git diff <file>`

1. 暂存区没有 stage 该文件：比较的是当前工作区和最新 commit 之间的区别；
2. 暂存区 staged 该文件：比较的是当前工作区和暂存区的区别；

`git diff --cached 文件名 `

比较暂存区和最新 commit 之间的区别。

## 解决冲突

1. `git status` 显示冲突文件
2. 使用文本编辑器处理 `<<<< ===== <<<<` 囊括起来的内容
3. `git add 冲突文件 ` 标识该文件冲突已解决

## 文件重命名

`git mv oldfilename newfilename`

同时 git 会自动把重命名这一变化提交到暂存区。

## 删除文件

`git rm <file>`

同时 git 会自动把删除文件这一变化提交到暂存区。

## 添加至暂存区

`git add <file>|<dir>`

## 从暂存区取消暂存

`git restore --staged <file>`

## 查看哈希对象的类型与内容

- 查看类型：`git cat-file -t 哈希值 `
- 查看内容：`git cat-file -p 哈希值 `

## 版本变更

`git reset` 接受 3 种参数：

- `--hard` ：丢弃暂存区和工作区的所有变化，回退到指定 commit_id，若未指定则为 HEAD；

```bash
# "相对路径"形式：回退到最新版本，即丢弃暂存区和工作区的所有变化
$ git reset --hard

# "相对路径"形式：回退到上上一个版本
$ git reset -- hard HEAD^^

# "绝对路径"形式：回退到某个版本
$ git reset --hard commit_id
```

- `--soft` ：
- `--mixed` ：

将工作区恢复至某个 commit，并删除该版本之后的所有 commit 信息。

分两种场景：

- **从现在穿越到过去**：`git reset --hard`
- **从过去穿越回现在**：执行 `git reset --hard` 会在 `git log` 中移除相应的 commit 导致丧失对应的 commit_id，需要依靠 `git reflog` 来找到被移除的 commit 的 commit_id。

`git checkout -- file or dir`

分两种情况：

- 文件修改已经添加到暂存区后，又作了修改：回到和暂存区内容一致（即上一次 `git add` ）
- 文件修改后还没有提交到暂存区：执行撤销操作后恢复成 **和最新 commit 内容一致**（即上一次 `git commmit` ）

如何理解：用暂存区或版本库里的版本覆盖当前工作区的版本，实现还原。

### 查看本地命令执行历史

`git reflog`

只能查看在本地执行过的命令，不会上传到远程服务器上。

主要用于 reset 出错时找回 commit_id。

### 撤销暂存区快照

`git reset file or dir` ：是 `git add file or dir` 的逆命令。

`Changes to be committed → Changes not staged for commit`

## 贮藏 stash

作用：将未完成的工作（涉及工作区和暂存区的改动）临时保存在一个栈中，可在任何时候、任何分支应用它们。

`git stash push -m 'message'`

git stash pop stash@{深度值}

`git stash save` ：废弃

git stash list

git stash drop stash@{深度值}

`git stash clear` ：清空

git stash show

git stash show -p

git stash apply stash@{深度值}

# 重写历史

参考：[Git Book-重写历史](https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E9%87%8D%E5%86%99%E5%8E%86%E5%8F%B2)

原则：“在满意之前不要推送你的工作”。

## 修改最后一次提交

本质: 用一个新的 commit 替换最新的 commit。

场景：

- 想修改最近一次提交的提交信息：
    - `git commit --amend` 在弹出的编辑器中进行修改。
- 想要修改最后一次提交的实际内容：
    - 直接在工作区进行修改, 然后用 `git commit --amend` **替换** 掉最后一次提交

如果你的修补是琐碎的（如修改了一个笔误或添加了一个忘记暂存的文件），那么之前的提交信息不必修改，你只需作出更改，暂存它们，然后通过以下命令避免不必要的编辑器环节即可：`git commit --amend --no-edit`.

## 修改非最后一次的历史提交

将想要修改的最近一次提交的 **父提交** 作为参数传递给 `git rebase -i` 命令。

```bash
pick f7f3f6d changed my name a bit
pick 310154e updated README formatting and added blame
pick a5f4a0d added cat-file

# Rebase 710f0f8..a5f4a0d onto 710f0f8
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup <commit> = like "squash", but discard this commit's log message
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified). Use -c <commit> to reword the commit message.
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

每个 commit 前的 command 表示需要对这个 commit 进行什么操作，设定好 command 后保存并退出编辑器。

```bash
$ git rebase -i HEAD~3
Stopped at f7f3f6d... changed my name a bit
You can amend the commit now, with

       git commit --amend

Once you're satisfied with your changes, run

       git rebase --continue
```

在工作区修改完文件后，通过 `git commit --amend` 替换该 commit 后，再通过 `git rebase --continue` 完成本次工作并到下一个需要修改的 commit 处。

## 重新排序提交/删除某提交

直接对行进行删除或重排序即可。

```console
pick f7f3f6d changed my name a bit
pick 310154e updated README formatting and added blame
pick a5f4a0d added cat-file
```

改为：

```console
pick 310154e updated README formatting and added blame
pick f7f3f6d changed my name a bit
```

# 分支管理

## 分支基础

### 列举所有分支（查看当前分支）

`git branch`

- 没有参数：列举所有本地分支
- `-a` ：列举本地和远程跟踪分支
- `-v` ：显示分支当前 commit 的 message
- `-vv` ：显示分支当前 commit 的 message 以及所关联的远程分支

`git log` 记录右上角提示

```bash
commit f2763629669f1c9fe834c82c18ee3d270dd06188 (HEAD -> master)
Author: Ryan <709224218@qq.com>
Date:   Sat Oct 17 16:25:03 2020 +0800

    renamed file1 to file

commit 0a38fea5a343e157f63773040f7b8d1374bff6ee
Merge: 90876d0 03a66e7
Author: RyanLiu <709224218@qq.com>
Date:   Sun Oct 11 23:40:07 2020 +0800

    solve conflicts
```

### 创建&删除本地分支

`git branch dev` ：创建 dev 分支

`git branch -d dev` ：完成 dev 合并后，删除不需要的 dev 分支（TODO- 合并？）

`git branch -D dev` ：未合并完 dev ，强制删除

### 切换分支

`git checkout dev`

### 创建并切换分支

`git checkout -b dev`

### 分支合并

#### merge

`git merge target_branch` ：将目标分支合并至当前分支

#### rebase

例如想把分支 A 合并到分支 B 上，操作流程：

1. 在分支 A 上执行 `git rebase B`：以 B 为基底执行变基
    1. 原理：找到两个分支的最近公共祖先 C，对比当前分支 A 相对于祖先 C 的多次 commit，并依次提取成多个临时文件，在目标分支 B 的最新节点上“回放 replay”这些临时文件，看上去就像 A 在基底 B 的 HEAD 基础上进行了多次 commit
    2. 结果：分支 A HEAD 指针领先于分支 B HEAD 指针多个 commit
2. 切换到分支 B 上执行：`git merge A`：让分支 B 的 HEAD 指针快进合并到分支 A 的 HEAD 指针

### 撤销分支合并

`git merge --abort` 或 `git rebase --abort`

### 冲突的本质

# 远程仓库

形式：`<remote_shortname> <url>`

## 分支类型

- remote branch：远程主机上的分支
- remote-tracking branch：形式为：`<remote>/<branch>` （仅用来表示远程分支状态、只读）
- tracking branch：根据 remote-tracking branch 创建出来的本地分支，remote-tracking branch 称为其 upstream branch （上游分支）
- local branch：普通本地分支，本地自己创建的分支

## 基于远程跟踪分支创建跟踪分支

`git checkout -b <branch> <remote>/<branch>`

```bash
$ git checkout -b bugfix origin/bugfix
Branch bugfix set up to track remote branch bugfix from origin.
Switched to a new branch 'bugfix'
```

最快捷方式，当本地没有 bugfix 分支时（需要先 `git fetch` 拉取更新跟踪分支）：

```bash
$ git checkout bugfix
Branch bugfix set up to track remote branch bugfix from origin.
Switched to a new branch 'bugfix'
```

## 设置上游分支

有时先创建了本地分支，此时其没有上游分支，需要为其绑定上游分支，方便之后直接 `git pull`

```
git branch -u origin/dev
```

绑定成功后，通过 `git config --list` 可以看到：

```
branch.dev.remote=origin
branch.dev.merge=refs/heads/dev
```

在推送时绑定

```
git push -u origin dev
```

## 克隆远程仓库

- `git clone <remote_url> newName`
- `git clone 远程仓库 ` 时 Git 会自动：
    - 将其添加为远程仓库并命名为 origin
    - 拉取所有数据
    - 在本地创建 `origin/master` 远程跟踪分支指向远程仓库的 `master` 分支
    - 在本地创建跟踪 `origin/master` 的 `master` 跟踪分支

## 列举所有远程仓库

`git remote` ：列举出所有远程仓库的 shortname

```bash
$ git remote
origin
```

`git remote -v` ：(verbose- 详细) 列出读写远程仓库对应的 name 和 url

```bash
$ git remote -v
origin	https://github.com/schacon/ticgit (fetch)
origin	https://github.com/schacon/ticgit (push)
```

## 查看某个远程仓库信息

```bash
$ git remote show origin
* remote origin
  Fetch URL: https://github.com/schacon/ticgit
  Push  URL: https://github.com/schacon/ticgit
  HEAD branch: master
  Remote branches:
    master                               tracked
    dev-branch                           tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)
```

## 添加远程仓库

`git remote add <remote_shortname> <url>`

## 移除远程仓库

`git remote remove <remote>`

## 拉取远程仓库更新

- `git fetch` ：更新远程跟踪分支
- 当拉取到新的远程分支时，Git 在本地创建一个只读的 `refs/remote/仓库名/new_branch` 跟踪引用，并不会自动生成对应的本地分支。可通过如下方式创建本地分支：

```bash
$ git checkout -b serverfix origin/serverfix
Branch serverfix set up to track remote branch serverfix from origin.
Switched to a new branch 'serverfix'
```

- `git pull` ：更新跟踪分支，并尝试自动合并入本地分支
- **Options related to merging**
    - `--no-commit` ：更新代码，但取消自动提交
    - 如果 `git pull` 拉取更新合并代码时没有发生冲突，Git 会自动 commit。如果想对本次 `git pull` 的 commit 做出调整则可以使用这个命令

## 向远程仓库推送

`git push <remote> <refspec>`

push 成功前提条件：分支可以通过 `fast-forward` 成功合并。

原理：

1. 如果没有指定 `<remote>`，Git 会读取 `branch.*.remote` 配置，若没有配置，则默认为 `origin`；
2. 如果没有指定 `<refspec>`，Git 会读取 `remote.*.push` 配置，若没有配置，则默认读取 `push.default` 配置；
3. 如果没有 `push.default` 配置，则默认 `push.default = simple` （将本地分支推送至同名的上游分支）

常见场景 1：`git push`

1. 没有指定 `remote`，默认为 `origin`
2. 没有指定 `<refspec>`，如果没有配置 `remote.*.push` 和 `push.default`，则推送至同名分支
3. 综上：`git push` 等价于 `git push origin 当前所在分支`

常见场景 2：`git push origin dev`

1. dev 表示 `<src>`，没有指定 `:<dst>`，故默认 `<dst>` 为和 `<src>` 同名分支
2. 即不管你当前在哪个分支，都向 origin 仓库推送本地 dev 至远端 dev 分支

`git push origin master` 和 `git push -u origin master` 区别？

在本地 master 和远端 master 没有建立关联的情况下，后一种写法可以在推送成功后建立本地 master 和远端 master 的映射关系。
