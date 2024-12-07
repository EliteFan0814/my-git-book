## git 常用命令

```bash
# 在本地把 git@github.com:xxx/xx.git 命名为 origin 来添加远程仓库地址
# 同一个本地仓库可以添加多个远程仓库
git remote add origin git@github.com:xxx/xx.git
git remote add origin2 git@github.com:yyy/yy.git
# 设置当前分支的上游分支为远程origin2的branch2分支 && 推送
git push -u origin master
git push -u origin2 branch2

# 更新远程分支列表
git remote update origin -p

# 修改本地分支名
git branch -m old_branch new_branch
```

## 拉取远程分支

## .gitignore 规则

```bash
# 所有空行或以 # 开头的行都会忽略
# 匹配模式可以以（/）开头防止递归。
# 匹配模式可以以（/）结尾指定目录。
# 要忽略指定模式以外的文件或目录，可以在模式前加上叹号（!）取反
# []中使用 - 表示范围： [0-9] 表示匹配所有 0 到 9 的数字
# [abc]匹配 a 或 b 或 c
# 问号（?）只匹配一个任意字符
# 星号（*）匹配零个或多个任意字符
# 使用两个星号（**) 表示匹配任意中间目录。比如 a/**/z 可以匹配 a/z 、 a/b/z 或 a/b/c/z 等

# 以下【当前目录】指.gitignore文件所在目录

# 忽略 .a 文件
*.a
# 但跟踪所有的 lib.a，即便你在前面忽略了 .a 文件
!lib.a
# 仅忽略当前目录下 TODO 文件/文件夹， 但不包括子目录下的 subdir/TODO
/TODO
# 递归忽略所有层级的 TODO 文件/文件夹
TODO
# 忽略任何目录下名为 build 的文件夹
build/
# 忽略当前目录下 build 文件夹下的所有文件，但不包括 build文 件夹本身
build/*
# 忽略当前目录下 doc/notes.txt, 不包括 doc/server/arch.txt
doc/*.txt
# 忽略在 doc 文件夹下的所有 .pdf 文件
doc/**/*.pdf
git checkout -b new-branch
# 做完一系列add commit 操作之后，设置上游仓库地址
git push --set-upstream origin new-branch
```

## git 回滚相关

```bash
# 显示提交日志
git log
# 查看HEAD 指向的历史清单
git reflog
# 回滚到指定版本（此操作危险，会直接把你add但没commit的文件删除掉）
git reset --hard 历史版本号
```

## 清空本地保存的用户名和密码(管理员权限)

```bash
git config --system --unset credential.helper
```

## git checkout xxx 注意事项

1. 从 A 分支检出到 B 分支时，A 分支处于 added modified 状态的文件也会一同转移到
   B 分支

## git diff 分析文件差异

**注意事项**：

1. git diff 比较的是【暂存区快照】和【工作目录】之间的差异
2. git diff --staged 比较的是【暂存区快照】和【最后一次提交的文件】之间的差异

## git rm 移除文件

1. git rm xxx 从已跟踪清单（暂存区）【移出文件】并从当前目录（磁盘）【删除文件】
2. git rm --cached xxx 从 已跟踪清单（暂存区）【移出文件】但并不从当前目录（磁盘
   ）删除文件

## git commit --amend 把修改合并到上一次提交

## git 设置系统代理
```git
<!-- 设置代理 -->
git config --global http.proxy http://127.0.0.1:7890 
git config --global https.proxy http://127.0.0.1:7890

<!-- 取消代理 -->
git config --global --unset http.proxy
git config --global --unset https.proxy
```