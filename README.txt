1.安装Git
我所使用的是Windows版本，下载地址：https://git-scm.com/download/win
安装完成后，在开始菜单里找到"Git"->"Git Bash"，会弹出 Git 命令窗口，你可以在该窗口进行 Git 操作。

2.创建版本库
版本库又名仓库，英文名repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。
首先创建一个新目录：$ mkdir learngit
接着通过git init命令把这个目录变成Git可以管理的仓库：
$ git init
Initialized empty Git repository in C:/Users/wanmengyu/test/.git/
.git的目录Git来跟踪管理版本库的，不可随意修改

3.把文件添加到版本库
首先写一个readme.txt文件：$ vim readme.txt
内容为：
Git is a version control system.
Git is free software.
注：写完后按Esc键，然后输入:wq进行对文件进行保存
接着用命令git add告诉Git，把文件添加到仓库：
$ git add readme.txt
然后用命令git commit告诉Git，把文件提交到仓库：
$ git commit -m "wrote a readme file"
[master (root-commit) 6a7e58f] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt

4.查看仓库当前的状态，对于修改过的文件，此时只是被修改，但并未准备提交修改，通过$ git add readme.txt 命令提交修改
 $ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")

5.查看修改之处
$ git diff readme.txt
warning: LF will be replaced by CRLF in readme.txt.
The file will have its original line endings in your working directory
diff --git a/readme.txt b/readme.txt
index 46d49bf..9247db6 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-Git is a version control system.
+Git is a distributed version control system.
 Git is free software.

6.提交修改之后再查看仓库状态
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   readme.txt

7.再提交文件
$ git commit -m "add diatributed"
[master d95574f] add diatributed
 1 file changed, 1 insertion(+), 1 deletion(-)

 8.版本回退
 首先修改readme.txt文件
 尝试提交：
 $ git add readme.txt
warning: LF will be replaced by CRLF in readme.txt.
The file will have its original line endings in your working directory
注：git add添加文件命令，实际上就是把文件修改添加到暂存区
$ git commit -m "append GPL"
[master 5656f50] append GPL
 1 file changed, 1 insertion(+), 1 deletion(-)
注：git commit -m提交修改命令

 9.用git log命令查看工作记录
 $ git log
commit 5656f5047c3ea5762c2fda6785c5fc36529970c5 (HEAD -> master)
Author: Keena-Y <945723464@qq.com>
Date:   Wed Mar 3 11:02:33 2021 +0800

    append GPL

commit d95574feca79c6a8669ad351b60f248928d77f98
Author: Keena-Y <945723464@qq.com>
Date:   Mon Mar 1 20:31:29 2021 +0800

    add diatributed

commit 6a7e58f84958aedb388b6cae94db250f862f2bcf
Author: Keena-Y <945723464@qq.com>
Date:   Mon Mar 1 16:06:51 2021 +0800

    wrote a readme file
注：此方法是显示全部，内容较多
$ git log --pretty=oneline
5656f5047c3ea5762c2fda6785c5fc36529970c5 (HEAD -> master) append GPL
d95574feca79c6a8669ad351b60f248928d77f98 add diatributed
6a7e58f84958aedb388b6cae94db250f862f2bcf wrote a readme file
注：此输出信息较少，直观明了

10.返回上一个版本
$ git reset --hard HEAD^
HEAD is now at d95574f add diatributed
注：返回上一个版本为HEAD^ ，返回上上个版本为HEAD^^
查看此时的文件是否为上一个版本
$ cat readme.txt
Git is a distributed version control system.
Git is free software.
注：返回哪一个版本只需在hard后面加上版本号，如append GPL版本就改为：$ git reset --hard 5656f
$  git reset --hard 5656f
HEAD is now at 5656f50 append GPL

11.git reflog用来记录你的每一次命令
$ git reflog
5656f50 (HEAD -> master) HEAD@{0}: reset: moving to 5656f
d95574f HEAD@{1}: reset: moving to HEAD^
5656f50 (HEAD -> master) HEAD@{2}: commit: append GPL
d95574f HEAD@{3}: commit: add diatributed
6a7e58f HEAD@{4}: commit (initial): wrote a readme file

12.创建一个record.txt文件
用git status查看两个文件的状态
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   readme.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        record.txt

no changes added to commit (use "git add" and/or "git commit -a")
Git非常清楚地告诉我们，readme.txt被修改了，而record.txt还从来没有被添加过，所以它的状态是Untracked。
现在，使用两次命令git add，把readme.txt和LICENSE都添加后，用git status再查看一下：
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   readme.txt
        new file:   record.txt
注：git add命令实际上就是把要提交的所有修改放到暂存区（Stage）
然后，执行git commit就可以一次性把暂存区的所有修改提交到分支。
$ git commit -m "learnning"
[master ff0478c] learnning
 2 files changed, 119 insertions(+)
 create mode 100644 record.txt
 查看状态
$ git status
On branch master
nothing to commit, working tree clean

13.管理修改
第一次修改 -> git add -> 第二次修改 -> git commit
当你用git add命令后，在工作区的第一次修改被放入暂存区，准备提交，但是，在工作区的第二次修改并没有放入暂存区，所以，git commit只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交。
每次修改，如果不用git add到暂存区，那就不会加入到commit中。

14.撤销修改
先查看此时的readme.txt文件
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributer under the GPL.
Git has a mutable index called stage.
1111
git checkout -- file可以丢弃工作区的修改：
命令git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：
一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
总之，就是让这个文件回到最近一次git commit或git add时的状态。
再次查看readme.txt文件
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributer under the GPL.
Git has a mutable index called stage.

当你已使用git add命令上传到暂存区时
使用命令git reset HEAD <file>可以把暂存区的修改撤销掉（unstage），重新放回工作区
如：git reset HEAD readme.txt

