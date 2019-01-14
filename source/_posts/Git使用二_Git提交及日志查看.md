---
title: Git使用二|Git提交及日志查看
url: git-usage-commit-log
date: 2019-01-14 22:00:00
tags: Git
categories: Version Control
---

上一节讲了如何使用`Git bash here`工具初始化仓库，并提交代码。但是我们日常使用中经常会对一个文件重复修改，提交或者提交错误，这时候就要查看日志，这一节就讲日常使用git时如何查看日志，并且根据日志id退回提交等常规操作。

<!--more-->

# 章节简介

上一节讲了如何使用`Git bash here`工具初始化仓库，并提交代码。但是我们日常使用中经常会对一个文件重复修改，提交或者提交错误，这时候就要查看日志，这一节就讲日常使用git时如何查看日志，并且根据日志id退回提交等常规操作。

## **git log** 成长轨迹

日志是查看和追溯项目进程的核心，比如查看提交记录，根据日志回退到历史节点，再比如追溯这个bug是谁搞出来的，如果没有log这个坑可能就你来背了。所以git可定要有日志来记录所有行为，接下来使用`git log`来查看日志：

``` bash
hhongwen@PC MINGW64 /d/Project/git-test (master)
$ git log
commit f8417c399321c41bc4bd7f1ef2774644847e8925 (HEAD -> master)
Author: hhongwen <hhhongwen@mail.cn>
Date:   Mon Jan 14 16:14:04 2019 +0800

    commit hello git file to master
```

对上面这个进行解析:

- `f8417c399321c41bc4bd7f1ef2774644847e8925`是本次提交的`id`， `id`是此次记录的唯一标示；
- 作者`Author: hhongwen <hhhongwen@mail.cn>`；
- 提交时间`Date:   Mon Jan 14 16:14:04 2019 +0800`；
- 提交描述`commit hello git file to master`；

下面让我们再次修改文件`hello-git.txt`，添加内容`edit this file again and commit it.`，然后保存提交再查看日志，顺便回顾一下上一节的命令：

1. `git status`查看状态
2. `git add <file>...`添加到暂存区
3. `git commit -m '...'`提交内容

然后查看日志如下：

``` bash
hhongwen@PC MINGW64 /d/Project/git-test (master)
$ git log
commit af6d585f24498ece88464813e6de8383e09758ad (HEAD -> master)
Author: hhongwen <hhhongwen@mail.cn>
Date:   Mon Jan 14 16:40:27 2019 +0800

    edit hello-git file

commit f8417c399321c41bc4bd7f1ef2774644847e8925
Author: hhongwen <hhhongwen@mail.cn>
Date:   Mon Jan 14 16:14:04 2019 +0800

    commit hello git file to master
```

**注意：**这时候会发现多了一条一个`edit hello-git file`的记录，想想仅仅提交两次都有这么多内容，正常一个项目几百次提交是很正常的，如果我们这么看会不会感觉很累？

出于上述想法所以git提供了一个简化信息的log可以添加参数`--pretty=oneline`查阅，这样就可以快速浏览日志内容，使用如下：

``` bash
hhongwen@PC MINGW64 /d/Project/git-test (master)
$ git log --pretty=oneline
af6d585f24498ece88464813e6de8383e09758ad (HEAD -> master) edit hello-git file
f8417c399321c41bc4bd7f1ef2774644847e8925 commit hello git file to master
```

git 还有一个`git reflog`命令来查看每一步的操作，记录了日志`id`的前7位和对应提交的描述，但是这里`git reflog`主要用途是来追溯git操作，后面会有介绍，这里还是建议使用`git log --pretty=oneline`命令。

``` bash
hhongwen@PC MINGW64 /d/Project/git-test (master)
$ git reflog
af6d585 (HEAD -> master) HEAD@{0}: commit: edit hello-git file
f8417c3 HEAD@{1}: commit (initial): commit hello git file to master
```

## **git diff** 今生今世

现在修改文件`hello-git.txt`，添加内容`use git diff look different`，由于记性不太好，改完文件出去上个厕所回来忘记了修改啥怎么办？这时候我们可以使用`git status`命令来查看状态都有哪写文件有修改，然后用`git diff`查看修改内容：

``` bash
hhongwen@PC MINGW64 /d/Project/git-test (master)
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   hello-git.txt

no changes added to commit (use "git add" and/or "git commit -a")

hhongwen@PC MINGW64 /d/Project/git-test (master)
$ git diff hello-git.txt
warning: LF will be replaced by CRLF in hello-git.txt.
The file will have its original line endings in your working directory
diff --git a/hello-git.txt b/hello-git.txt
index 6449117..d943aeb 100644
--- a/hello-git.txt
+++ b/hello-git.txt
@@ -1,3 +1,5 @@
 hello git, this is my first use git.

 edit this file again and commit it.
+
+use git diff check different.
```

这时候能看到`+use git diff check different`提示，添加这样一段内容，然后看了看提交上去了。这时候一拍大腿提交错了，上完厕所脑袋短路怎么办，接下来撤回此次提交。

## **git reset** 回到过去

当我们提交过后，要撤回到历史版本首先查看log日志：

``` bash
hhongwen@PC MINGW64 /d/Project/git-test (master)
$ git log --pretty=oneline
8585bc7448e460451cedf0d0ee1de6bd68361c95 (HEAD -> master) add diff content
af6d585f24498ece88464813e6de8383e09758ad edit hello-git file
f8417c399321c41bc4bd7f1ef2774644847e8925 commit hello git file to master
```

这里能看到有三次提交，我们最后一次`8585bc7448e460451cedf0d0ee1de6bd68361c95 (HEAD -> master) add diff content`提交，可以看到内容里有`HEAD`表示是当前版本，要撤回到`edit hello-git file`只需要用`git reset --hard HEAD^`命令即可。

``` bash
hhongwen@PC MINGW64 /d/Project/git-test (master)
$ git reset --hard HEAD^
HEAD is now at af6d585 edit hello-git file
```

提示现在是在`id`为`af6d585`开头的`edit hello-git file`版本，如果我们要撤回两个版本怎么做，只需要在`HEAD^`再添加一个`^`即可，即`git reset --hard HEAD^^`。如果要是退回多个版本则用数字表示`git reset --hard HEAD~10`。

这时候我们再查看日志已经剩两个版本了，成功回到了历史版本：

``` bash
hhongwen@PC MINGW64 /d/Project/git-test (master)
$ git log --pretty=oneline
af6d585f24498ece88464813e6de8383e09758ad (HEAD -> master) edit hello-git file
f8417c399321c41bc4bd7f1ef2774644847e8925 commit hello git file to master
```

## **git reset** 回到未来

其实文章开始就已经讲述了id是git log的唯一标示，那么我们也可以根据id来切换版本。现在我们查看log已经找不到`add diff content`回退前的id了，怎么办？记得文章中间讲过一个命令`git reflog`，其实它记录了我们每一步的操作，让我们看一下和`git log --pretty=oneline`有哪些不同：

``` bash
hhongwen@PCL MINGW64 /d/Project/git-test (master)
$ git reflog
af6d585 (HEAD -> master) HEAD@{0}: reset: moving to HEAD^
8585bc7 HEAD@{1}: commit: add diff content
af6d585 (HEAD -> master) HEAD@{2}: commit: edit hello-git file
f8417c3 HEAD@{3}: commit (initial): commit hello git file to master
```

这里能看到`log id`从`f8417c3` --> `af6d585` -->  `8585bc7` --> `af6d585`，也就是中间`af6d585` -->  `8585bc7` --> `af6d585`这部分使我们回退的操作，那么我们就可以根据`8585bc7`这个`id`回到未来`add diff content`版本。

``` bash
hhongwen@PC MINGW64 /d/Project/git-test (master)
$ git reset --hard 8585bc7
HEAD is now at 8585bc7 add diff content
```

**注意：** 大家看到我用的`id`是`8585bc7`，所以我们切换版本不用写全部的`id`，只需要**前7位**就可以了。

这时候提示`HEAD is now at 8585bc7 add diff content`已经处于`add diff content`版本，然后再查看日志：

``` bash
hhongwen@PC MINGW64 /d/Project/git-test (master)
$ git log --pretty=oneline
8585bc7448e460451cedf0d0ee1de6bd68361c95 (HEAD -> master) add diff content
af6d585f24498ece88464813e6de8383e09758ad edit hello-git file
f8417c399321c41bc4bd7f1ef2774644847e8925 commit hello git file to master
```

这样我们就回到了最终的状态，是不是很神奇，所以一开始就介绍了**日志是查看和追溯项目进程的核心**，所以如果有谁删了你的代码找回去跟他理论，这锅咱们不背^_^

## 小结

本节主要一查看日志为主线，然后涉及可版本切换、内容查看等命令，在这过程中使用最多的还是上一节提到的`git add <file>...`和`git commit -m '...'`和`git status`三个命令，只是本节没有再列出来，而且`git log`也是我们以后经常会用的命令，下面做一下总结。

- `git log` 和 `git log --pretty=oneline` 查看日志及版本。
- `git diff`文件有哪些修改
- 通过`HEAD`来指定版本，使用命令`git reset --hard id`来切换版本（`id`是指log中的`id`）
- `git reflog`查看所有的操作过程，进行历史追溯

## **如果有疑问可以留言，一起讨论**
