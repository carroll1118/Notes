@[toc]
# 什么是Git?
* Git 是一个开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。
* Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。
* Git 与常用的版本控制工具 CVS, Subversion 等不同，它采用了分布式版本库的方式，不必服务器端软件支持。
# Git 的一般工作流程
* 克隆 Git 资源作为工作目录。
* 在克隆的资源上添加或修改文件。
* 如果其他人修改了，你可以更新资源。
* 在提交前查看修改。
* 提交修改。
* 在修改完成后，如果发现错误，可以撤回提交并再次修改并提交。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200322165720811.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70)
# Git常用命令
* Git有三大区（工作区、暂存区、版本库）以及几个状态（untracked、unstaged、uncommited）
----
##### 1.从github克隆文件到本地
```java
	git clone  文件地址
```
##### 2.查看文件目录
```java
	git status
	//git status 以查看在你上次提交之后是否有修改。
```
##### 3.将本地文件提交到暂存区
```java
	git add [文件名/文件夹/通配符]
```
##### 4.从暂存区提交文件到master分支
```java
	git commit -m "第一次提交"
	//将文件写入缓存区并提供提交注释
```
##### 5.查看日志
```java
	git  log
```
##### 6.查看提交记录
```java
	git show  "commit_ID"
```
##### 7.从master分支push到github仓库
```java
	git push
```
##### 8.回滚到上一步
```java
	git  reset 
```
##### 9.删除添加到暂存区的文件
```java
		1. 仅仅删除暂存区里的文件
		git rm --cache 文件名
```
```java
		2. 删除暂存区和工作区的文件
		git rm -f 文件名
```
##### 10.删除错误提交的commit

```java
	//仅仅只是撤销已提交的版本库，不会修改暂存区和工作区
	git reset --soft 版本库ID
```
```java
	//仅仅只是撤销已提交的版本库和暂存区，不会修改工作区
	git reset --mixed 版本库ID
```
```java
	//彻底将工作区、暂存区和版本库记录恢复到指定的版本库
	git reset --hard 版本库ID   
```
##### 11.git diff
* 执行 git diff 来查看执行 git status 的结果的详细信息。
* git diff 命令显示已写入缓存与已修改但尚未写入缓存的改动的区别。git diff 有两个主要的应用场景。
	* 尚未缓存的改动：`git diff`
	* 查看已缓存的改动： `git diff --cached`
	* 查看已缓存的与未缓存的所有改动：`git diff HEAD`
	* 显示摘要而非整个 diff：`git diff --stat`

**你知道的越多，你不知道的越多。
有道无术，术尚可求，有术无道，止于术。
如有其它问题，欢迎大家留言，我们一起讨论，一起学习，一起进步**


