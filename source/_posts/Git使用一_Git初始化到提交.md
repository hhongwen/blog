---
title: Git使用一|Git初始化到提交
url: git-summary-of-usage
date: 2019-01-10 22:00:00
tags: Git
categories: Version Control
---

> Git是一个免费的开源 分布式版本控制系统，旨在快速，高效地处理从小型到大型项目的所有事务。Git 易于学习， 占地面积小，具有闪电般快速的性能。它超越了Subversion，CVS，Perforce和ClearCase等SCM工具，具有廉价本地分支，便捷的临时区域和 多个工作流程等功能。

<!--more-->

> Git是一个免费的开源 分布式版本控制系统，旨在快速，高效地处理从小型到大型项目的所有事务。Git 易于学习， 占地面积小，具有闪电般快速的性能。它超越了Subversion，CVS，Perforce和ClearCase等SCM工具，具有廉价本地分支，便捷的临时区域和多个工作流程等功能。

由于公司项目从SVN切换到Git，所以有必要了解和学习一下Git基础命令介绍和使用场景，所以一切都从实用出发。

这里默认大家都安装了Git，如果没有安装点击这里查看[Git](https://git-scm.com/)，安装请参考[Git安装](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137396287703354d8c6c01c904c7d9ff056ae23da865a000)

# 步入正题，挑干的来

## Git Bash Here

`Git Bash Here`是Windows安装完git自带的git管理工具，我是这么理解的。而且初学者想了解透git的使用还是从基础开始，Linux&Mac用户直接在终端打开就可以敲git命令了，而Windows的`Git Bash Here`就类似Linux&Mac的终端，在任意文件夹鼠标右键即可看到。所以接下来的所有操作都是在`Git Bash Here`用git命令的方式完成，我认为学好git命令才能更好的使用git管理工具，接下来讲git基础使用。

## 初始化

新建文件夹`git-test`作为以后学习git的初始仓库，既然有仓库了，那么怎么才能使用`git`呢？右键点击`Git Bash Here`然后开始第一个命令`git init`

``` bash
hhongwen@PC MINGW64 /d/Project/git-test
$ git init
Initialized empty Git repository in D:/Project/git-test/.git/

hhongwen@PC MINGW64 /d/Project/git-test (master)
$
```

完成后即可在`git-test`文件夹内看到生成`.git`的文件夹，会记录以后所有git的操作日志，分支以及版本管理等。这个时候git仓库就已经创建好了，这时候会有上面看到`master`提示，代表这是git仓库的master分支，下面就开始git之旅。

## 创建文件

输入命令`touch hello-git.txt`创建一个文件`hello-git.txt`，然后对文件进行编辑`vim hello-git.txt`，点击`i`进行编辑，输入一句话`hello git, this is my first use git.`，然后点击`ESC`输入`:x`保存退出(这里需要会linux的vim或vi命令，如果不了解直接用编辑器打开修改文件即可)

``` bash
hhongwen@PC MINGW64  /d/Project/git-test (master)
$ touch hello-git.txt

hhongwen@PC MINGW64  /d/Project/git-test (master)
$ vim hello-git.txt
```

## Git状态

编写完`hello-git.txt`文件处于未被git跟踪的状态，此时使用`git status`命令来查看状态，这时候会看到下面的内容。看看`git status`时都提示了哪些信息：

- **On branch master**提示处于master分支
- **No commits yet**没有提交的内容
- **Untracked files**未跟踪文件及需要提交的内容，这时候会提示 **`use "git add <file>..." to include in what will be committed`**

``` bash
hhongwen@PC MINGW64  /d/Project/git-test (master)
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        hello-git.txt

nothing added to commit but untracked files present (use "git add" to track)
```
> **注：** 这时已经可以看出 **`git status`** 就是查看当前仓库的状态，也是最长用的命令，它会提示你当前分支处于什么状态，接下来概要做哪些内容，后期当我们遇到其他问题时在解释相关的内容该如何处理，所以要养成使用`git status`的习惯。

## Git提交

### git add

按着上述的提示我们需要将`hello-git.txt`文件添加到**暂存区**使用`git add <file>...`命令，然后再用`git status`查看状态，你会看到不一样的提示：

``` bash
hhongwen@PC MINGW64 /d/Project/git-test (master)
$ git add hello-git.txt

hhongwen@PC MINGW64 /d/Project/git-test (master)
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   hello-git.txt
```

这时候提示 **`use "git rm --cached <file>..." to unstage`** 用来删除**暂存区的文件**，不会删除本地的文件，然后再编辑和添加到暂存区。但是我们不需要删除而是提交，所以使用`git commit`命令提交。

> **注：**上面提示的是`git rm --cached <file>...`有`cached`参数，如果不加`cached`参数会同时删除本地的文件，所以使用时要注意。

### git commit

将暂存区的文件提交到分支使用`git commit`命令，我们提交的时候需要写上备注，这时候用`git commit -m '...'`来提交。

``` bash
hhongwen@PC MINGW64 /d/Project/git-test (master)
$ git commit -m 'commit hello git file to master'
[master (root-commit) 0d80201] commit hello git file to master
 1 file changed, 1 insertion(+)
 create mode 100644 hello-git.txt
```

这时候已经将`hello-git.txt`文件提交到了本地分支，我们学习的第一步即将结束，这时候不要忘记使用`git status`查看一下状态。

``` bash
hhongwen@PC MINGW64 /d/Project/git-test (master)
$ git status
On branch master
nothing to commit, working tree clean
```
`nothing to commit, working tree clean`代表分支没有需要提交的。

## 小结

这章主要讲了如何用`git init`初始化git仓库，然后编辑内容并提交到仓库，后期`git add <file>...`和`git commit -m '...'`和`git status`也是最常用的命令，这些命令将伴随后期所有的Git学习，需要多加练习。

### 下一章

通过本章已经了解了Git的如何从初始化到提交，这只是第一步，但是一般我们都是重复的修改和提交，怎么能查看整个任务的进程，请看下一章[Git提交及日志查看](https://hhongwen.cn/20190114/git-usage-commit-log)

## **如果有疑问可以留言，一起讨论**
