#**git教程总结**#
4/8/2018 10:10:20 PM 
----------
## **创建版本库：** ##
    git init 
    git add 
    git commit
初始化一个Git仓库，使用git init命令。
添加文件到Git仓库，分两步：
第一步，使用命令git add <file>，注意，可反复多次使用，添加多个文件；
第二步，使用命令git commit，完成。


    git status 
    git diff



要随时掌握工作区的状态，使用git status命令。
如果git status告诉你有文件被修改过，用git diff可以查看修改内容。


##** 版本回退： **##

    git log 显示从最近到最远的提交日志（含版本号）
    git log --pretty=oneline 一行显示一次提交
    git reset --hard HEAD^ 退回到上一个版本
    git reset --hard HEAD^^ 退回到上两个版本
    git reset --hard HEAD~100 退回到上100个版本
    git reset --hard 3628164（退回到版本号为3628164的版本）
    git reset --hard commit_id
    git log
    git reflog
现在总结一下
HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。
穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。
要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。


## **工作区和暂存区:** ##
- stage(index)
- master 、Head

----------

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向
  master的一个指针叫HEAD。
前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：
第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；
第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。
因为我们创建Git版本库时，Git自动为我们创建了唯一一个master分支，所以，现在，git commit就是往master分支上提交更改。
你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。


## **管理修改：** ##
修改的内容必须要git add 才能被git commit，否则没有被git add的内容在git commit时不会被提交。
git add将修改的内容放在stage（暂存区）中，git commit则将暂存区中的文件提交上去。
所以如果不对修改的内容文件git add ，git commit就不会将修改的内容提交。


## **撤销修改：** ##
场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。
场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。
场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。


## **删除文件：** ##
一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用rm命令删了：
    rm test.txt
现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令git rm删掉，并且git commit：

    git rm test.txt
    git commit -m "remove test.txt"

现在，文件就从版本库中被删除了。
另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：

    git checkout -- test.txt

git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。
命令git rm用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。


## **远程仓库：** ##
第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

    ssh-keygen -t rsa -C "youremail@example.com"

你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。
如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。
第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：
然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容
点“Add Key”，你就应该看到已经添加的Key
为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送


## **添加远程库：** ##
要关联一个远程库，使用命令


    git remote add origin git@server-name:path/repo-name.git；

关联后，使用命令

    git push -u origin master

第一次推送master分支的所有内容；
此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；


从远程库克隆：
要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。
Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。


> https://github.com/for-Carina/gitskills.git

> git@github.com:for-Carina/gitskills.git


## **创建与合并分支：** ##
Git鼓励大量使用分支：

    查看分支：git branch
    创建分支：git branch <name>
    切换分支：git checkout <name>
    创建+切换分支：git checkout -b <name>
    合并某分支到当前分支：git merge <name>
    删除分支：git branch -d <name>


## **解决冲突：** ##
当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

    git merge --no-ff -m "merge with no-ff" dev

用

    git log --graph

命令可以看到分支合并图。


## **分支管理策略：** ##

    git merge --no-ff -m "merge with no-ff" dev

在实际开发中，我们应该按照几个基本原则进行分支管理：
首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；
你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。
合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。
合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

## **bug分支：** ##
Git还提供了一个stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作
首先确定要在哪个分支上修复bug，假定需要在master分支上修复，就从master创建临时分支

    git checkout master
    git checkout -b issue-101

现在修复bug，然后提交

    git add readme.txt
    git commit -m "fix bug 101"

修复完成后，切换到master分支，并完成合并，最后删除issue-101分支

    git checkout master
    git merge --no-ff -m "merged bug fix 101" issue-101
    git branch -d issue-101

现在，是时候接着回到dev分支干活了！

    git checkout dev
    git stash list

工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：
一是用

    git stash apply

恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；
另一种方式是用

    git stash pop

恢复的同时把stash内容也删了
你可以多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash，用命令

    git stash apply stash@{0}


## **Feature分支：** ##

    开发新功能：git checkout -b feature-vulcan
假设开发完毕，提交：

    git add vulcan.py
    git commit -m "add feature vulcan"
    切回dev，准备合并：git checkout dev

新功能突然取消：

    git branch -d feature-vulcan

销毁失败。Git友情提醒，feature-vulcan分支还没有被合并，如果删除，将丢失掉修改，如果要强行删除，需要使用命令

    git branch -D feature-vulcan


## **多人协作：** ##
推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上

    git push origin master

如果要推送其他分支，比如dev，就改成

    git push origin dev

但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？
master分支是主分支，因此要时刻与远程同步；
dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。
总之，就是在Git中，分支完全可以在本地自己藏着玩，是否推送，视你的心情而定！
多人协作时，大家都会往master和dev分支上推送各自的修改。
现在，模拟一个你的小伙伴，可以在另一台电脑（注意要把SSH Key添加到GitHub）或者同一台电脑的另一个目录下克隆

    git clone git@github.com:michaelliao/learngit.git

当你的小伙伴从远程库clone时，默认情况下，你的小伙伴只能看到本地的master分支。
现在，你的小伙伴要在dev分支上开发，就必须创建远程origin的dev分支到本地，于是他用这个命令创建本地dev分支

    git checkout -b dev origin/dev

你的小伙伴已经向origin/dev分支推送了他的提交，而碰巧你也对同样的文件作了修改，并试图推送
你会推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法也很简单，Git已经提示我们，先用

    git pull

把最新的提交从origin/dev抓下来，然后，在本地合并，解决冲突，再推送
git pull也失败了，原因是没有指定本地dev分支与远程origin/dev分支的链接，根据提示，设置dev和origin/dev的链接：

    git branch --set-upstream dev origin/dev

再pull：

    git pull

这回git pull成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的解决冲突完全一样。解决后，提交，再push

因此，多人协作的工作模式通常是这样：
首先，可以试图用

    git push origin branch-name

推送自己的修改；
如果推送失败，则因为远程分支比你的本地更新，需要先用

    git pull

试图合并；
如果合并有冲突，则解决冲突，并在本地提交；
没有冲突或者解决掉冲突后，再用

    git push origin branch-name

推送就能成功！
如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令

    git branch --set-upstream branch-name origin/branch-name

这就是多人协作的工作模式，一旦熟悉了，就非常简单。

小结
查看远程库信息，使用

    git remote -v

本地新建的分支如果不推送到远程，对其他人就是不可见的；
从本地推送分支，使用

    git push origin branch-name

如果推送失败，先用git pull抓取远程的新提交；
在本地创建和远程分支对应的分支，使用

    git checkout -b branch-name origin/branch-name

本地和远程分支的名称最好一致；
建立本地分支和远程分支的关联，使用

    git branch --set-upstream branch-name origin/branch-name

从远程抓取分支，使用git pull，如果有冲突，要先处理冲突


## **标签** ##
## **创建标签和操作标签：** ##
命令

    git tag <name>
用于新建一个标签，默认为HEAD，也可以指定一个commit id：`git tag v0.9 6224937`

    git tag -a <tagname> -m "blablabla..."

可以指定标签信息；

    git tag -s <tagname> -m "blablabla..."

可以用PGP签名标签；
命令

    git tag

可以查看所有标签。
命令

    git push origin <tagname>

可以推送一个本地标签；
命令

    git push origin --tags

可以推送全部未推送过的本地标签；
命令

    git tag -d <tagname>

可以删除一个本地标签；
命令

    git push origin :refs/tags/<tagname>

可以删除一个远程标签。


GitHub：
在GitHub上，可以任意Fork开源仓库；
自己拥有Fork后的仓库的读写权限；
可以推送pull request给官方仓库来贡献代码。


忽略特殊文件：
在Git工作区的根目录下创建一个特殊的.gitignore文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件
不需要从头写.gitignore文件，GitHub已经为我们准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：

> https://github.com/github/gitignore

忽略文件的原则是：
忽略操作系统自动生成的文件，比如缩略图等；
忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的.class文件；
忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。
最后一步就是把.gitignore也提交到Git，就完成了！当然检验.gitignore的标准是git status命令是不是说working directory clean
使用Windows的童鞋注意了，如果你在资源管理器里新建一个.gitignore文件，它会非常弱智地提示你必须输入文件名，但是在文本编辑器里“保存”或者“另存为”就可以把文件保存为.gitignore了。
有些时候，你想添加一个文件到Git，但发现添加不了，原因是这个文件被.gitignore忽略了

    git add App.class
The following paths are ignored by one of your .gitignore files:
App.class
Use -f if you really want to add them.
如果你确实想添加该文件，可以用-f强制添加到Git：

    git add -f App.class
或者你发现，可能是.gitignore写得有问题，需要找出来到底哪个规则写错了，可以用git check-ignore命令检查：

    git check-ignore -v App.class

.gitignore:3:*.class    App.class
Git会告诉我们，.gitignore的第3行规则忽略了该文件，于是我们就可以知道应该修订哪个规则。


