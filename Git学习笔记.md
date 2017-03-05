#Git学习笔记——廖雪峰Git教程
##1.安装配置

###1.1.配置身份
```bash
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```
>`—global `参数会将配置应用到本机的所有Git仓库，当然也可以只定义到某个仓库

----
##2.创建版本库

###2.1.将目录编程Git可以管理的仓库： 
```shell
$ git init
Initialized empty Git repository in /Users/michael/learngit/.git/
```
###2.2.将文件添加到仓库 
```shell
$ git add readme.txt
```
###2.3.将修改的文件提交 
```shell
$ git commit -m "wrote a readme file"
 [master (root-commit) cb926e7] wrote a readme file
  1 file changed, 2 insertions(+)
  create mode 100644 readme.txt
``` 

----
##3.版本基本控制

###3.1.查看当前仓库的状态 
```shell
$ git status
# On branch master
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#    modified:   readme.txt
#
no changes added to commit (use "git add" and/or "git commit -a”)
```
###3.2.查看修改文件的差异 
```shell
$ git diff readme.txt 
diff --git a/readme.txt b/readme.txt
index 46d49bf..9247db6 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-Git is a version control system.
+Git is a distributed version control system.
Git is free software.
```
###3.3.查看修改的历史记录 
```bash
 $ git log
 commit 3628164fb26d48395383f8f31179f24e0882e1e0
 Author: Michael Liao <askxuefeng@gmail.com>
 Date:   Tue Aug 20 15:11:49 2013 +0800
 
  append GPL
 
 commit ea34578d5496d7dd233c827ed32a8cd576c5ee85
 Author: Michael Liao <askxuefeng@gmail.com>
 Date:   Tue Aug 20 14:53:12 2013 +0800
 
  add distributed
 
 commit cb926e7ea50ad11b8f9e909c05226233bf755030
 Author: Michael Liao <askxuefeng@gmail.com>
 Date:   Mon Aug 19 17:51:55 2013 +0800
 
	wrote a readme file
```
>`—pretty=oneline`可以将显示变为一行

###3.4.版本回退
>回退到上一个commit版本，在Git中`HEAD`表示当前版本，上一个版本为`HEAD^`，上上个版本是`HEAD^^`，以此类推，也可以用-数字的方式，例如之前100个版本`HEAD-100` 
>Git 内部有个指向当前版本的`HEAD`指针，回退版本实际就是在移动`HEAD` 指针，以指向想要的版本，指向修改后同时工作区的文件也会相应的更新
```bash
$ git reset --hard HEAD^
HEAD is now at ea34578 add distributed
```
相关概念：
- 工作区（Working Directory）
- 版本库（Repository）
>工作区下面有个`.git`文件夹，这个`.git`文件夹就是Git的版本库，版本库中最重要的就是称为`Stage`（或Index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的`HEAD`指针
>`git add` 是将文件添加到Stage中去
>`git commit`提交修改，实际是把Stage中的所有内容提交到当前分支，因为现在没有还有创建其他分支，所以`git commit`提交到了Git自动为我们创建也是当前唯一的一本分支`mater`上.
>流程就是先在工作区进行创作修改，完成后添加到`Stage`中，然后commit将`Stage`中暂存的修改提交到到Branch中
 **重点：**
- `git commit`只会提交Stage中的修改，工作区的修改不会被提交
- **因此Git管理的是修改，而非文件**

###3.5.撤销修改
>`git checkout -- file` 把文件在工作区的修改全部撤销
>**只撤销工作区的修改，提交的Stage中的修改无法通过`git checkout`撤销**
>回退到最近一次的`git commit` 或`git add` 的状态
```bash
vsc-python git:(master) ✗ git checkout -- readme.txt
```
>如果已经提交到了`Stage`,可以使用`git reset HEAD file`回退到工作区

###3.6.删除文件
>如果在工作区`rm file`删除了文件，会造成工作区与版本库内容不一致，如果确实要在版本库上删除该文件，正确的做法应该是使用`git rm file`删除文件，然后`git commit` 提交修改
	>**如果删错了，那么需要使用`git checkout -- file`撤销修改即可恢复文件**

----
##4.远程仓库

###4.1.添加远程仓库

