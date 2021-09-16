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
