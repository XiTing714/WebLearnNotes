# Git

## 介绍



* 把目录变为仓库:  ``git init``
* 把文件**添加**到仓库: ``git add filename``
  * 用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区
* 把文件**提交**到仓库: ``git commit -m "提交说明"``
  * 用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支
  * 每次修改，如果不用`git add`到暂存区，那就不会加入到`commit`中
* 

#### 版本退回

* `HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。
* 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。
* 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。

#### 撤销修改

* 场景1：当你改乱了工作区某个文件的内容，想直接**丢弃工作区的修改**时，用命令`git checkout -- file`。

* 场景2：当你不但改乱了工作区某个文件的内容，**还添加到了暂存区时**，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`，就回到了场景1，第二步按场景1操作。

* 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考[版本回退](https://www.liaoxuefeng.com/wiki/896043488029600/897013573512192)一节，不过前提是没有推送到远程库。

#### 删除文件

* 一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用`rm`命令删了。要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`：

  ```
  $ git rm test.txt
  rm 'test.txt'
  
  $ git commit -m "remove test.txt"
  [master d46f35e] remove test.txt
   1 file changed, 1 deletion(-)
   delete mode 100644 test.txt
  
  ```

* 删错了，没关系(前提是曾经提交到版本库)，可以恢复: ``$ git checkout -- test.txt``



#### 远程仓库

###### 情况一: 先有本地仓库

登陆GitHub，创建一个新的仓库(GitHub上的仓库就是远程仓库)

* 要关联一个远程库，使用命令`git remote add origin git@github.com:XiTing714/learngit.git`

* 关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容
  * 由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令

* 此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改

分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！

###### 情况二: 从远程库克隆

``$ git clone git@github.com:michaelliao/gitskills.git``

或``git clone https://github.com/schacon/ticgit``

#### 分支管理

###### 分支管理策略

* 在实际开发中，我们应该按照几个基本原则进行分支管理：
  * 首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
  * 那在哪干活呢？干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；
  * 你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了。

###### Bug分支(pending)

* 修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

* 当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，修复后，再`git stash pop`，回到工作现场；

* 在master分支上修复的bug，想要合并到当前dev分支，可以用`git cherry-pick <commit>`命令，把bug提交的修改“复制”到当前分支，避免重复劳动。

###### feature分支

* 开发一个新feature，最好新建一个分支；

* 如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除

###### 多人协作

* 多人协作时，大家都会往`master`和`dev`分支上推送各自的修改。

* 现在，模拟一个你的小伙伴，可以在另一台电脑（注意要把SSH Key添加到GitHub）或者同一台电脑的另一个目录下克隆：

  ```
  $ git clone git@github.com:michaelliao/learngit.git
  Cloning into 'learngit'...
  remote: Counting objects: 40, done.
  remote: Compressing objects: 100% (21/21), done.
  remote: Total 40 (delta 14), reused 40 (delta 14), pack-reused 0
  Receiving objects: 100% (40/40), done.
  Resolving deltas: 100% (14/14), done.
  ```

* 现在，你的小伙伴要在`dev`分支上开发，就必须创建远程`origin`的`dev`分支到本地，于是他用这个命令创建本地`dev`分支：

  ```
  $ git checkout -b dev origin/dev
  ```

* 现在，他就可以在`dev`上继续修改，然后，时不时地把`dev`分支`push`到远程：

  ```
  $ git add env.txt
  
  $ git commit -m "add env"
  [dev 7a5e5dd] add env
   1 file changed, 1 insertion(+)
   create mode 100644 env.txt
  
  $ git push origin dev
  Counting objects: 3, done.
  Delta compression using up to 4 threads.
  Compressing objects: 100% (2/2), done.
  Writing objects: 100% (3/3), 308 bytes | 308.00 KiB/s, done.
  Total 3 (delta 0), reused 0 (delta 0)
  To github.com:michaelliao/learngit.git
     f52c633..7a5e5dd  dev -> dev
  ```



* 你的小伙伴已经向`origin/dev`分支推送了他的提交，而碰巧你也对同样的文件作了修改，并试图推送：

  ```
  $ cat env.txt
  env
  
  $ git add env.txt
  
  $ git commit -m "add new env"
  [dev 7bd91f1] add new env
   1 file changed, 1 insertion(+)
   create mode 100644 env.txt
  
  $ git push origin dev
  To github.com:michaelliao/learngit.git
   ! [rejected]        dev -> dev (non-fast-forward)
  error: failed to push some refs to 'git@github.com:michaelliao/learngit.git'
  hint: Updates were rejected because the tip of your current branch is behind
  hint: its remote counterpart. Integrate the remote changes (e.g.
  hint: 'git pull ...') before pushing again.
  hint: See the 'Note about fast-forwards' in 'git push --help' for details.
  ```



* 推送失败，因为远程分支比你的本地更新，解决办法也很简单，Git已经提示我们，先用`git pull`把最新的提交从`origin/dev`抓下来，然后，在本地合并，解决冲突，再推送：

  ```
  $ git pull
  There is no tracking information for the current branch.
  Please specify which branch you want to merge with.
  See git-pull(1) for details.
  
      git pull <remote> <branch>
  
  If you wish to set tracking information for this branch you can do so with:
  
      git branch --set-upstream-to=origin/<branch> dev
  ```

  `git pull`也失败了，原因是没有指定本地`dev`分支与远程`origin/dev`分支的链接，根据提示，设置`dev`和`origin/dev`的链接：

  ```
  $ git branch --set-upstream-to=origin/dev dev
  Branch 'dev' set up to track remote branch 'dev' from 'origin'.
  ```

  再pull：

  ```
  $ git pull
  Auto-merging env.txt
  CONFLICT (add/add): Merge conflict in env.txt
  Automatic merge failed; fix conflicts and then commit the result.
  ```

  这回`git pull`成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的[解决冲突](http://www.liaoxuefeng.com/wiki/896043488029600/900004111093344)完全一样。解决后，提交，再push：

  ```
  $ git commit -m "fix env conflict"
  [dev 57c53ab] fix env conflict
  
  $ git push origin dev
  Counting objects: 6, done.
  Delta compression using up to 4 threads.
  Compressing objects: 100% (4/4), done.
  Writing objects: 100% (6/6), 621 bytes | 621.00 KiB/s, done.
  Total 6 (delta 0), reused 0 (delta 0)
  To github.com:michaelliao/learngit.git
     7a5e5dd..57c53ab  dev -> dev
  ```



* 因此，多人协作的工作模式通常是这样：

  1. 首先，可以试图用`git push origin <branch-name>`推送自己的修改；
  2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
  3. 如果合并有冲突，则解决冲突，并在本地提交；
  4. 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！

  如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。

  这就是多人协作的工作模式，一旦熟悉了，就非常简单。