# GIT学习笔记

[本文来自廖雪峰的 git 教程。](https://www.liaoxuefeng.com/wiki/896043488029600)

#### git 基础

>git是使用C语言编写的。

　　`git init` 把当前目录改成可以被git管理的目录，当前目录就成为了**工作区**

　　`git add <文件名>` 把文件添加到**暂存区**

　　`git commit -m "提交说明"` 把文件提交到**版本库**

　　　　　　`-m 本次提交的说明`

> 为什么Git添加文件需要add，commit一共两步呢？因为commit可以一次提交很多文件，所以你可以多次add不同的文件

　　`git status` 当前仓库的状态

　　`git diff <文件名>` 查看修改的内容

　　`git log` 查看提交的历史记录

　　　　　　`git log --pretty=oneline` 

---

#### 恢复版本

HEAD是一个指针，每提交一次版本，指针就自动指向最新的版本，每个版本都在log中出现。所以回退的时候只需要让指针指向之前的版本就可以。

```shell
git reset --hard HEAD^ 			# 回退上一个版本
git reset --hard HEAD^^ 		# 回退到前两个版本
git reset --hard HEAD~100 		# 回退到100个版本之前
git reset --hard <commitID> 	# 回退指定的版本
```

　　`git reflog` 查看命令历史，以便确定要回到未来的哪个版本

　　`git checkout -- <文件名>`  撤销**工作区**的更改，还原暂存区到工作区

　　`git reset HEAD <文件名>`  撤销**暂存区**的更改，还原HEAD到工作区

　　`git rm <文件名>`  删除**版本库**和**工作区**的文件 并且不需要`git add`可直接`git commit`

　　　　`git rm --cached <文件名>`从**版本库**中删除文件，但是本地（**工作区**）的文件不会删除。可以用于添加.gitignore而忽略某一个或几个添加到仓库的文件时使用。

---

#### 分支

　　`git checkout -b <dev>`  创建一个分支名为dev，并且切换到dev分支。

相当于以下的两个命令：

```shell
$ git branch <dev>  # 创建分支dev
$ git checkout <dev>  # 切换到dev分支
```

　　`git branch` 查看当前在哪个分支上操作

　　`git merge <dev>`  把指定分支（dev）的内容合并到当前的分支

　　　　　　`git merge --no-ff -m "提交说明" <dev>` 保留分支提交记录，即使删除分支也会在log中出现。便于合作时使用

　　`git branch -d <dev>`  删除分支（dev）

　　　　　　`git branch -D <name>` 强制删除一个没有合并的分支

> Git鼓励使用分支完成任务，合并后再删除分支，因为更加的安全。

最新版本的Git提供了新的`git switch`命令来切换分支：

```shell
git switch -c <dev>  		# 创建并切换到新的(dev)分支
git switch <master> 		# 切换到已有的(master)分支
```

　　`git log --graph`  命令可以看到分支合并图

　　　　　　`git log --graph --pretty=oneline --abbrev-commit`

> master分支应该只用来发布正式版本，自己的工作应该在自己的分支上完成。

　　`git stash` 将现在手中的工作放置，去其他分支管理别的任务

　　`git stash list` 查看有几个搁置的任务

　　`git stash pop` 回到工作现场

　　`git cherry-pick <commitID>` 把master上某个更改复制到当前分支上（如修复bug的commit）

每开发一个新功能，最好新建一个新分支，最后再合并到自己的工作分支。

