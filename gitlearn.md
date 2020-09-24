[TOC]

<h1><center>git学习笔记</center></h1>



### 1.在Windows上安装git

官网下载并安装，安装完后，在开始菜单找到 “Git”->"Git Bash",弹出一个类似命令行窗口的东西，则说明安装成功。

安装完后进行用户设置

Git 是分布式版本控制系统，因此，每台机器都必须自报家门：名字 和 邮箱

`git config --global`命令 表示这台机器上的所有仓库都用这个配置

```git
$ git config --global user.name "LD"
$ git config --global user.email "649166610@qq.com"
```

### 2.Git 基本操作

#### 2.1 创建版本库

版本库又名仓库【repository】，可以理解为是一个目录，这个目录下的文件都可以被git管理起来。

##### 创建版本库命令

```git
$ mkdir learngit
$ cd learngit
$ pwd
```

`pwd`命令用于显示当前目录 创建目录的和切换目录的命令与`cmd`相同

**通过`git init`命令将当前目录变成Git可以管理的目录**

```git
$ git init
```

初始化后当前目录会出现一个`.git`的目录【默认隐藏】`ls -ah`命令可以看见当前目录下的文件

**把文件添加到版本库**

第一步，用命令`git add filename`告诉git，把文件提交到仓库 `git add dir/`  添加整个文件夹

第二步，用命令`git commit -m "explanation text" `-m 后面输入的是本次提交的说明文字

#### 2.2 时光机穿梭

`git diff filename`查看具体修改的内容 在文件add、cmmit之前才可以查看

`git status`命令查看仓库的当前状态

##### 版本回退

`git log`命令显示从最近到最远的提交日志的详细信息 `git log --pretty=oneline`显示简略信息

在git中，用`HEAD`表示当前版本，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，往上100个版本可以写成`HEAD~100`

回退到上一个版本`git reset --hard HEAD^`  回到当前版本 `git reset --hard commit id` commit id不用写全，写前几位就行 如[4831d]

`git reflog` **记录每一次commit id 查看命令历史**

##### 工作区和暂存区

工作区（Working Directory）

Git 和其他版本控制系统如SVN的一个不同之处就是有暂存区的概念

工作区指的是当前git目录 如【gitMarkdown】

版本库（Repository）

工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库

版本库中有很多东西。最重要的及时称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针`HEAD`

前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：

第一步：是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区

第二步：是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支

##### 管理修改

`$ cat filename`查看文件内容

`$ git commit`只提交添加到暂存区内的修改，工作区内的修改如果未被添加到暂存区是不会被提交到分支上的

##### 撤销修改

如果没有将修改提交到暂存区则直接`git checkout -- filename`  类似于`Ctrl +Z`

如果已经将修改提交到了暂存区则`git reset HEAD filename`  `git reset`既表示回退版本也可以将暂存区的修改撤销到工作区，工作区的 修改则同上

如果已经`git commit`修改 则可以直接回退版本`git reset --hard HEAD^`

##### 删除文件

删错了则可以直接从版本库里恢复【前提是版本库里还有】`git checkout -- checkout.txt`

确实要删除则可以从版本库里删除，并提交`git rm checkout.txt   git commit`

#### 2.3远程仓库

本地仓库和远程仓库的传输是通过SSH加密的，所以，首先需要创建SSH Key 在用户主目录下，看看有没有`.ssh`目录，如果有在看看这个目录下有没有`id_rsa私钥不能泄露 id_rsa.pub公钥` 

创建SSH Key：命令

`$ ssh-keygen -t rsa -C "649166610@qq.com"  `

##### 添加远程仓库

先有本地库再关联远程库

`git remote add github git@github.com:lvdong05/gitMarkdown.git`

将本地库的所有内容推送到远程库上

`git push -u github master` `git push`实际上是把当前分支master推送到远程，由于远程仓库是空的，第一次推送master分支时，加上了`-u`参数，Git不但会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取就可以简化命令

##### 从远程仓库克隆

从零开始开发，一般是先创建远程仓库，然后从远程仓库克隆

`git clone git@github.com:lvdong05/gitskills.git`

#### 2.4 分支管理

##### 创建与合并分支

查看分支命令`git branch`

在Git里，master分支是主分支【分支也就是一条时间线】

新建分支并切换到新建分支命令`git switch -c ld`  相当于 `git branch ld`和`git switch ld`这两个命令  也可以用这个命令`git checkout -b ld`

合并某分支到当前分支命令

`git merge ld` 【ld是指ld这个分支】 这个是 ***快进模式 Fast-forward***  的合并也就是直接把master 指向 ld 的提交

删除分支	`git branch -d ld`

##### 解决冲突

多个分支同时修改同一内容并提交，可能会造成冲突，要先手动解决冲突后，再提交

`git log --graph` 查看合并分支图 `git log --graph --pretty=oneline --abbrev-commit`

在当前分支下不能删除当前分支【即我不能删我自己】

##### 分支管理策略

通常，合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。

强制禁用`Fast forward`模式，Git会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息

`git merge --no-ff -m "merge with no-ff" dev`

`--no-ff`表示禁用`Fast forward`  本次合并要创建一个新的commit，所以要交`-m`参数

##### 分支策略

在实际开发中，我们应该按照几个基本原则进行分支管理：

>+ `master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能再上面干活
>+ `dev`分支是平时用来开发的分支，也就是说，`dev`分支是不稳定的，当要发布版本时，比如要发布v1.0版本，则把`dev`分支合并到`master`上，在`master`分支上发布v1.0版本
>+ 团队开发时，团队组员都在`dev`上干活，每个人都有自己的分支，然后不时往`dev`分支上合并

##### Bug分支

在Git中，由于分支过于强大，因此，每个bug都可以通过一个新的临时分支来修复，修复后合并分支，然后将临时分支删除。

当当前分支上的工作还未完成无法提交。同时又需要到另一个分支上去修复bug时，可以使用`git stash`命令 将当前分支上的工作**储藏**，然后就可以切换到另一个分支上去修复bug。当再次切换到当前分支时可以用 `git stash list`命令来查看**储藏**的工作，同时也可以通过`git stash pop`来恢复工作【注：`git stash apply`命令也可以用来恢复工作，不过同时还需要`git stash drop`来删除**储藏**的工作】

同样的bug存在于不同的分支上，如果当前分支也有同样的bug 要修复则可以在当前分支上用`git cherry-pick commit Id` 这个命令，这样可以把其他分支上的一个特定提交，复制到当前分支

##### Feature分支

软件开发中，总有无穷无尽的新功能要不断添加进来

添加新功能时，为了不让一些实验性质的代码，把主分支搞乱，所以每添加一个新功能，最好新建一个feature分支，在上面开发完，完成后，合并，最后删除该feature分支。

#### markdown语法在

### 3.怎么创建大纲和目录

#### 怎么创建大纲和目录

#### 怎么创建目录

