---
description: Git
---

## 常用命令解释

```bash
echo "# learn-git-1" >> README.md
git init
git add README.md
git commit -m "first commit"
# 创建 main 分支 -M 表示：重命名分支，即使新分支名称已存在
git branch -M main
# 添加远程仓库 将其名称命名为 origin (一般默认)
git remote add origin git@github.com:EliteFan0814/learn-git-1.git
# 你也可以起另外的名字 设置远程仓库名为 fp
# git remote add fp git@github.com:EliteFan0814/learn-git-1.git

# git push -u 即 git push --set-upstream
git push -u origin main

```

使用 `git push -u <remote name> <branch name>` 上传分支，并且跟踪远程分支

## 拉取远程分支

1. 已经拉取过代码，想拉取别的分支到本地

```git
git fetch

方式1. 本地创建并切换到分支
git checkout -b branchName origin/branchName

方式2. 拉取远程分支到本地
git pull origin branchName
```

git fetch 的目的是获取远程最新仓库信息，避免出现类似：  
fatal: 'origin/ac_branch' is not a commit and a branch 'ac_branch' cannot be
created from it  
这样的错误

## 删除分支

```git
删除本地分支
git branch -d branchName

删除远程分支
git push origin --delete branchName
```

## git 切换到历史版本

1. 使用 git log 查看历史提交记录

   |    命令    |       实现操作       |
   | :--------: | :------------------: |
   | 查看下一行 |    回车 方向键下     |
   | 查看上一行 |    y 键 方向键上     |
   | 查看下一页 | 空格键或 PageDown 键 |
   | 查看上一行 |   b 键或 PageUp 键   |
   |    退出    |         q 键         |

2. 复制目标历史的 commit 值，使用命令：  
   git checkout 4f0c057fd5e3b3f3cf5b55c6b58a436052f10510（目标历史 commit 值）

3. 根据提示，使用 git switch -c 分支名 给历史版本创建新分支

```bash
PS D:\myProjects\manage> git log
commit 5c102460e6f7e2680052572fad504aa5c8da5772 (HEAD -> master, origin/master, origin/HEAD)
Author: codebug <codebug@outlook.com>
Date:   Fri Apr 2 10:05:16 2021 +0800

    codebug add

commit 96730aa52dbd430b380ef62966bbe188b9752164
Author: yhl <1254776513@qq.com>
Date:   Thu Apr 1 18:24:24 2021 +0800

    add

:...skipping...
commit 5c102460e6f7e2680052572fad504aa5c8da5772 (HEAD -> master, origin/master, origin/HEAD)
Author: codebug <codebug@outlook.com>
Date:   Fri Apr 2 10:05:16 2021 +0800

    codebug add
PS D:\myProjects\manage> git checkout 4f0c057fd5e3b3f3cf5b55c6b58a436052f10510
Note: switching to '4f0c057fd5e3b3f3cf5b55c6b58a436052f10510'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false
PS D:\myProjects\manage> git switch -c changeBefore
Switched to a new branch 'changeBefore'
```

## 创建新仓库

```bash
echo "# base-weapp-components" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:EliteFan0814/base-weapp-components.git
git push -u origin main
```

## 存入已有仓库

```bash
git remote add origin git@github.com:EliteFan0814/base-weapp-components.git
git branch -M main
git push -u origin main
```

## 上传本地新分支到远程仓库

```bash
git checkout -b new-branch
# 做完一系列add commit 操作之后，设置上游仓库地址
git push --set-upstream origin new-branch
```

## 更改默认分支名称后的本地操作

```bash
git branch -m watch main
git fetch origin
git branch -u origin/main main
git remote set-head origin -a
```
