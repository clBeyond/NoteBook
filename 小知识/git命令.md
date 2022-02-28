git命令

1, git init 在指定文件夹下执行，将该文件夹初始化为git管理的仓库
2, git add readme.txt 将文件添加到仓库，可以添加多个文件
3, git commit -m "描述" 提交修改
4, git status 查看仓库状态，看看是否有修改
5, git diff 查看不同，有修改后执行可以显示出修改的地方
6, git log 查看修改日志，确定要回到哪个版本。git log --pretty=oneline 精简一些
7, git reset --hard HEAD^ 回退到上次版本。HEAD^^ 上上个版本 HEAD～100 上一百个版本
8, git reflog 查看已经执行过的每一条命令，从中找到commit id 用来恢复版本
9, Git管理的文件分为：工作区，版本库，版本库又分为暂存区stage和暂存区分支master(仓库)

工作区>>>>暂存区>>>>仓库

git add 把文件从工作区>>>>暂存区，git commit把文件从暂存区>>>>仓库，

git diff 查看工作区和暂存区差异，

git diff --cached查看暂存区和仓库差异，

git diff HEAD 查看工作区和仓库的差异，

git add 的反向命令git checkout，撤销工作区修改，即把暂存区最新版本转移到工作区，

git commit 的反向命令git reset HEAD，就是把仓库最新版本转移到暂存区。

git diff 时是分为两种情况的：暂存区为空和暂存区不为空。

首先我们明确知道git diff是比较工作区和暂存区的文件的，如果此时暂存区为空，那么稍微有点不同，即：

 1 暂存区为空使用git diff：因为此时暂存区为空，此时使用git diff同样也是比较工作区和仓库，即和使用git diff HEAD结果相同

2 暂存区不为空使用git diff:因为此时暂存区不为空，此时使用git diff比较的就是工作区和暂存区

10 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git 

reset HEAD <file>，就回到了场景1，第二步按场景1操作。

11, git remote add 远程连接名 git@github.com:githubid/远程仓库名.git 建立远程连接

12， git remote -v 查看远程连接信息

13， git remote rm 远程连接名。删除不用的远程连接

14， git push -u origin master  第一次推送master分支的所有内容
     Git push origin master  推送最新修改

15， git branch dev 创建dev分支
     	git checkout/switch dev 指针切换到dev
     Git checkout -b dev 创建并切换

16 git branch 查看当前分支

17 git merge 合并分支内容 合并后可以删除dev分支。git branch -d dev


