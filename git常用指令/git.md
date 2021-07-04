![image-20210413211052869](git.assets/image-20210413211052869.png)

## git分支常用命令

```git
# 列出所有本地分支
git branch

# 列出所有远程分支
git branch -r

# 新建一个分支，但依然停留在当前分支
git branch [branch-name]

# 新建一个分支，并切换到该分支
git checkout -b [branch]

# 合并指定分支到当前分支
$ git merge [branch]

# 删除分支
$ git branch -d [branch-name]

# 删除远程分支
$ git push origin --delete [branch-name]
$ git branch -dr [remote/branch]


# 展示历史操作图
git log --oneline --graph
```

## git 常用提交指令

```
#查看指定文件状态
git status [filename]

#查看所有文件状态
git status

# git add .                  添加所有文件到暂存区
# git commit -m "消息内容"    提交暂存区中的内容到本地仓库 -m 提交信息

# 第二次提交的时候可以连着提交
git commit -a -m '消息内容'


```

## git 其他指令

```
git diff：比较工作区和暂存区差异

git diff --cached :比较暂存区和版本区的差异
# master就和master分支的比
git diff master:比较工作区域和版本区的差异
```

### git 恢复删除指令

```
# 暂存区和版本区保持一致
git rest HEAD <file>

# 删除暂存区的文件
git rm <file> --cached

# 显示所有提交过的版本信息
git log 
# 恢复版本区指定版本的内容到工作区

git rest --hard <version>

# 可以查看所有分支的所有操作记录（包括已经被删除的 commit 记录和 reset 的操作）
git reflog
```

