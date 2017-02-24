---
layout: post
title:  "常用的一些git命令"
date:   2017-01-22 19:05:00 +0800
categories: "git"
tag: ["commands"]
---


* content
{:toc}

## 一些常用的git命令：
### 1. 克隆一个 repository 到本地
clone： git clone [url] [repository] [local repository name]

### 2. 保存修改  
####  git add -A   
保存所有的修改
####  git add .    
 保存新的添加和修改，但是不包括删除
####  git add -u   
保存修改和删除，但是不包括新建文件。

### 3. git目录管理
#### git init
当你本地创建了一个工作目录，你可以进入这个目录，使用'git init'命令进行初始化，Git以后就会对该目录下的文件进行版本控制。
#### git remote add [服务器别名] [url]
初始化本地目录后， 这时候如果你需要将它放到远程服务器上，可以在远程服务器上创建一个目录，并把可访问的URL记录下来，此时你就可以利用'git remote add'命令来增加一个远程服务器端。例如：git  remote  add  origin  git://github.com/someone/another_project.git
上面的命令就会增加URL地址为'git: //github.com/someone/another_project.git'，名称为origin的远程服务器，以后提交代码的时候只需要使用 origin别名即可。
#### git remote -v
查看远程仓库
#### git remote rm [服务器别名]
删除远程仓库
#### git remote set-url --push [服务器别名] [newUrl]
修改远程仓库
#### git pull [remoteName] [localBranchName]
拉取远程仓库
git push [remoteName] [localBranchName]
推送远程仓库

### 4. 分支(branch)操作相关命令
#### git branch
查看本地分支
#### git branch -r
查看远程分支
#### git branch [name]
创建本地分支, 注意新分支创建后不会自动切换为当前分支
#### git checkout [name]
切换分支
#### git checkout -b [name]
创建新分支并立即切换到新分支
#### git branch -d [name]
删除分支, -d选项只能删除已经参与了合并的分支，对于未有合并的分支是无法删除的。如果想强制删除一个分支，可以使用-D选项
#### git merge [name]
合并分支, 将名称为[name]的分支与当前分支合并
#### git push origin [name]
创建远程分支(本地分支push到远程)
#### git push origin :heads/[name] 或 $ gitpush origin :[name]
删除远程分支

*创建空的分支：(执行命令之前记得先提交你当前分支的修改，否则会被强制删干净没得后悔)
$git symbolic-ref HEAD refs/heads/[name]
$rm .git/index
$git clean -fdx
