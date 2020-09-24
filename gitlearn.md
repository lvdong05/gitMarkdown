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

**创建版本库命令**

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



#### markdown语法在

### 3.怎么创建大纲和目录

#### 怎么创建大纲和目录

#### 怎么创建目录

