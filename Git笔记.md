GIT使用
1.	 Git是什么？
是世界上最先进的分布式版本控制系统

2.	安装 
官网安装，全局安装，安装完成后需要下面两条命令：	
git config --global user.name "Your Name"
 git config --global user.email "email@example.com"
这两条命令告诉Git使用者是谁
3.	创建一个版本库
第一步：创建一个空目录
$ mkdir learngit //创建learngit这个文件夹
$ cd learngit//进入这个文件夹
$ pwd//查看当前目录
/Users/michael/learngit
第二步：把这个目录变成Git可以管理的仓库
git init
4.	把文件添加到版本库中
第一步：把文件添加到仓库中
$ git add readme.txt
第二步：把文件提交到仓库
git commit -m "wrote a readme file"//-m后面输入的是本次提交的说明
[master (root-commit) eaadf4e] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
注意：commit可以一次提交很多文件，你可以多次add不同的文件，一起提交。
5.	git status命令可以让我们时刻掌握仓库当前的状态。
用git diff这个命令能看看具体修改了什么内容
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
$ git diff readme.txt 
diff --git a/readme.txt b/readme.txt
index 46d49bf..9247db6 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-Git is a version control system.
+Git is a distributed version control system.
 Git is free software.
git log命令查看历史记录，git log命令显示从最近到最远的提交日志, 可以加上
--pretty=oneline参数：
$ git log --pretty=oneline
1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master) append GPL
e475afc93c209a690c39c13a46716e8fa000c366 add distributed
eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0 wrote a readme file
注意：你看到的一大串类似1094adb...的是commit id（版本号）
6.	Git可以使用git reset命令来回退版本
$ git reset --hard HEAD^
HEAD is now at e475afc add distributed
注意：上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。
Git reset命令也可以指定去到哪个版本
$ git reset --hard 1094a
HEAD is now at 83b0afe append GPL
注意：1094a指的是版本号，不用写全，Git能自己找。
7.	Git提供了一个命令git reflog用来记录你的每一次命令：
$ git reflog
e475afc HEAD@{1}: reset: moving to HEAD^
1094adb (HEAD -> master) HEAD@{2}: commit: append GPL
e475afc HEAD@{3}: commit: add distributed
eaadf4e HEAD@{4}: commit (initial): wrote a readme file

8.	Git 划分了工作区和暂存区，工作区就是你在电脑里能看到的目录，工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD
把文件往Git版本库里添加的时候，是分两步执行的：
第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；
第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。
需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。
9.	Git跟踪并管理的是修改，而非文件。用git diff HEAD -- readme.txt命令可以查看工作区和版本库里面最新版本的区别：
10.	命令git checkout -- readme.txt意思就是，把文件在工作区的修改全部撤销，这里有两种情况：
一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
总之，就是让这个文件回到最近一次git commit或git add时的状态。
用命令git reset HEAD <file>可以把暂存区的修改撤销掉（unstage），重新放回工作区
11.	删除时：用rm命令在文件管理器中把没用的文件删了
用命令git rm从版本库中删除该文件并且git commit
      如果一个文件已经被提交到版本库可以很轻松地把误删的文件恢复到最新版本：
$git checkout -- test.txt

12.	先有本地库，后有远程库的时候，如何关联远程库。要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；
关联一个远程库时必须给远程库指定一个名字，origin是默认习惯命名；
关联后，使用命令git push -u origin master第一次推送master分支的所有内容；
此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；
13.	我们从零开发，那么最好的方式是先创建远程库，然后，从远程库克隆。首先，登陆GitHub，创建一个新的仓库，下一步是用命令git clone克隆一个本地库
git clone git@github.com:michaelliao/gitskills.git
Cloning into 'gitskills'...
remote: Counting objects: 3, done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 3
Receiving objects: 100% (3/3), done.

14.	Git分支
首先，我们创建dev分支，然后切换到dev分支：
$ git checkout -b dev
Switched to a new branch 'dev'
git checkout命令加上-b参数表示创建并切换，相当于以下两条命令：
$ git branch dev//创建分支dev
$ git checkout dev//切换到dev分支上
Switched to branch 'dev'
可以用git branch命令查看当前分支：
$ git branch
* dev //带*号的就是当前分支
  master
git branch命令会列出所有分支，当前分支前面会标一个*号。
然后，我们就可以在dev分支上正常提交，比如对readme.txt做个修改，加上一行：
Creating a new branch is quick.
然后提交：
$ git add readme.txt 
$ git commit -m "branch test"
[dev b17d20e] branch test
 1 file changed, 1 insertion(+)
现在，dev分支的工作完成，我们就可以切换回master分支：
$ git checkout master //切换分支
Switched to branch 'master'
现在，我们把dev分支的工作成果合并到master分支上：
$ git merge dev //合并分支
Updating d46f35e..b17d20e
Fast-forward
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
git merge命令用于合并指定分支到当前分支
合并完成后，就可以放心地删除dev分支了：
$ git branch -d dev //删除分支
Deleted branch dev (was b17d20e).
另外
创建并切换到新的dev分支，也可以使用：
$ git switch -c dev
直接切换到已有的master分支，可以使用：
$ git switch master
小结
Git鼓励大量使用分支：
查看分支：git branch
创建分支：git branch <name>
切换分支：git checkout <name>或者git switch <name>
创建+切换分支：git checkout -b <name>或者git switch -c <name>
合并某分支到当前分支：git merge <name>
删除分支：git branch -d <name>

