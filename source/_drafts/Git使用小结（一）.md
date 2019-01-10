---
title: Git使用小结（一）
url: git-summary-of-usage
date: 2019-01-10 12:00:00
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
Administrator@PC-20180913ERXL MINGW64 /d/Project/git-test (master)
$ touch hello-git.txt

Administrator@PC-20180913ERXL MINGW64 /d/Project/git-test (master)
$ vim hello-git.txt
```

## 查看状态