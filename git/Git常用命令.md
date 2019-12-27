[TOC]
# Git常用命令
## 1、基本命令
`set LESSCHARSET=utf-8` --IDE Terminal乱码(idea自带的操作git窗口)

`git clone 分支名` --将分支克隆到本地

`git stash` --将修改暂时放到暂存区，在a分支改了东西，想先切到b分支，可以先用这个命令

`git stash pop` --从暂存区弹出，从b再切回a，用这个命令把改的恢复

`git checkout` 分支名或文件名 --切换到分支

`git pull` --拉取最新文件（更新）

//这三个通常一起使用 用于提交代码到远程分支
`git add` 文件名 --添加文件（用于后续提交）
`git commit -m "提交备注信息"` --提交文件到本地分支
`git push` --推送到远程分支

`git cherry-pick 版本号` --将a分支的某版本合并到b版本时可以用这个

## 2、添加了多余文件后，已经add未commit时撤销添加
`git reset HEAD 文件名` --撤销某文件
`git reset HEAD` --全撤销

## 3、已经commit还未push时，回退版本号
`git reset --mixed 要回退到哪个版本号`（本地代码还保留着）
`git reset --hard 要回退到哪个版本号`（本地代码不保留）

## 4、push后回退撤销
对于已经把代码push到远程仓库，你回退本地代码其实也想同时回退远程仓库的代码，回滚到某个指定的版本，本地、远程分支代码保持一致。
你要用revert命令

`git revert`用于反转提交,执行revert命令时要求工作树必须是干净的.
`git revert`用一个新提交来消除一个历史提交所做的任何修改.

>revert 之后你的本地代码会回滚到指定的历史版本，这时你再 git push 既可以把线上的代码更新.(这里不会像reset造成冲突的问题)
>revert 使用，需要先找到你想回滚版本唯一的commit标识代码，可以用 git log 或者在adgit搭建的web环境历史提交记录里查看.

`git revert c011eb3c20ba6fb38cc94fe5a8dda366a3990c61`

通常,前几位即可`git revert c011eb3`
`git revert`是用一次新的commit来回滚之前的commit
`git reset`是直接删除指定的commit
看似达到的效果是一样的,其实完全不同.

上面我们说的如果你已经push到线上代码库, reset 删除指定commit以后,你git push可能导致一大堆冲突.但是revert 并不会.

如果在日后现有分支和历史分支需要合并的时候,reset 恢复部分的代码依然会出现在历史分支里.但是revert 方向提交的commit 并不会出现在历史分支里.

>reset 是在正常的commit历史中，删除了指定的commit，这时 HEAD 是向后移动了，而 revert 是在正常的commit历史中再commit一次，只不过是反向提交，他的 HEAD 是一直向前的.

## 5、alias简化git命令
`git config --global alias.别名 原始命令`

如：
`git config --global alias.st status`
`git config --global alias.ck checkout`
`git config --global alias.ct commit`

以后再使用是就可以直接用简化后的命令了，`git st`
对于代码管理员来说，每天都要合很多版本，如执行`git cherry-pick 版本号`
若将cherry-pick简化为cp就会少敲很多字母。

## 6、查看提交日志记录
一般情况下：
1、`git log`--查看所有提交log
可以增加参数达到不同的日志显示效果
2、`git log --author 用户名`--查询某用户提交记录
效果：

3、`git log --author 用户名 --grep "关键词"` 显示某个用户提交的，含有某关键词的记录
效果：

4、可选命令`--reverse` 不加这个的话默认是按时间顺序，最新提交的先显示，加这个参数的话最早提交的先显示。
5、`--oneline`简化显示 在3的基础上简化，只显示一行，版本号简化

效果：

6、复杂点的
`git log --graph --pretty=format:'%Cred%h%Creset - %C(yellow)%d%Creset %s %Cgreen(%cr) %C(blue)<%an>%Creset' --abbrev-commit --date=relative`

## 7、Git查看仓库地址
我们的代码仓库多了，或者用的时间长了，很长时间没有clone过了，很容易忘了clone的URL，这时候我们可以用这个命令

`git remote -v`

查看到当前仓库的URL