所有的版本控制系统(GIT），其实只能跟踪文本文件的改动，比如TXT文件，网页，所有的程序代码等等。图片、视频这些二进制文件，虽然也能由版本控制系统管理，但没法跟踪文件的变化，只能把二进制文件每次改动串起来，也就是只知道图片从100KB改成了120KB，但到底改了啥，版本控制系统不知道，也没法知道。
   Microsoft的Word格式是二进制格式，版本控制系统是没法跟踪Word文件的改动的，真正使用版本控制系统，就要以纯文本方式编写文件。
   Unix的哲学是“没有消息就是好消息”。
创建版本库：
   初始化一个Git仓库，使用git init命令。
    可以通过Git命令touch readme.txt创建文件。
    添加文件到Git仓库，分两步：
第一步，使用命令git add <file>，注意，可反复多次使用，添加多个文件；
第二步，使用命令git commit完成，-m后面输入的是本次提交的说明，可以输入任意内容，最好是有意义的。
修改文件：
   git status命令可以让我们时刻掌握仓库当前的状态。
   git diff可以查看修改内容。
   提交修改和提交新文件有两步，第一步是git add，然后运行git commit查看仓库的状态，第二步git commit。
时空穿梭：
   使用命令git reset --hard commit_id在版本的历史之间穿梭。
   用git log可以查看提交历史，以便确定要回退到哪个版本。
   用git reflog查看命令历史，以便确定要回到未来的哪个版本。
   Windows里面要用git reset --hard “HEAD^”。
工作区和暂存区：
工作区：在电脑里能看到的目录，比如learngit这个文件夹。
暂存区：Git版本库里称为stage或者index的暂存区。
git add命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行git commit就可以一次性把暂存区的所有修改提交到分支。
每次修改，如果不add到暂存区，那就不会加入到commit中。

撤销修改：
  git checkout -- file可以丢弃工作区的修改。
  git checkout -- readme.txt意思就是把readme.txt文件在工作区的修改全部撤销，这里有两种情况：
   一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态。
一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
总之，就是让这个文件回到最近一次git commit或git add时的状态。
  git reset HEAD file（readme.txt）可以把暂存区的修改撤销掉（unstage），重新放回工作区。
  git reset命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用HEAD时，表示最新的版本。
小结：
场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。
场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。
场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。
删除文件：
git rm用于删除一个文件。只能恢复文件到最新版本，会丢失最近一次提交后你修改的内容。
远程仓库：
要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；
关联后，使用命令git push -u origin master第一次推送master分支的所有内容；
此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改。
分支管理：
查看分支：git branch
创建分支：git branch <name>
切换分支：git checkout <name>
创建+切换分支：git checkout -b <name>
合并某分支到当前分支：git merge <name>
删除分支：git branch -d <name>

git log --graph命令可以看到分支合并图

分支管理的基本原则：
Master应该非常稳定，仅用来发布新版本，平时不能在上面干活。
Dev是不稳定的，在dev分支上面干活。需要时合并分支。

合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。
BUG修复：
git stash可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作。
   git stash pop恢复stash的同时把内容也删了。

  丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。
新建标签：
git tag <name>用于新建一个标签，默认为HEAD，也可以指定一个commit id。
git tag -a <tagname> -m "blablabla..."可以指定标签信息；
git tag -s <tagname> -m "blablabla..."可以用PGP签名标签；
git tag可以查看所有标签。
删除标签:
git push origin <tagname>可以推送一个本地标签.
git push origin --tags可以推送全部未推送过的本地标签.
git tag -d <tagname>可以删除一个本地标签.
git push origin :refs/tags/<tagname>可以删除一个远程标签.

--global参数是全局参数，也就是这些命令在这台电脑的所有Git仓库下都有用。
git config --global alias.(st status)配置别名
