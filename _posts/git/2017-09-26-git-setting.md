---
layout: post
title:  "Git的相关设置"
date:   2017-09-26 15:05:00 +0800
categories: "git"
tag: ["config"]
---


* content
{:toc}

解决Git冲突的建议和工具

##### 1. 设置建议  
当遇到冲突时，可以在命令行或者其他可视化工具中输入“git mergetool”来初始化合并活动。在“.gitconfig”中用“merge.tool”变量来设置 Git 中自己喜欢的冲突解决软件，比如用 KDiff3 的可能会这样填写”.gitconfig” 的 merge 部分：
```js
[merge]
tool="kdiff3"
```
上面的语句等价于在命令行输入以下命令：
```js
git config --global merge.tool kdiff3
```

##### 2. 在冲突标记中显示（分支）共同的祖先
用下面的设置来改进冲突标记使其也显示（分支）共同祖先：
```js
git config --global merge.conflictstyle diff3
```

##### 3. 合并时使用“耐心”算法  
如果文件内容很长（比如一个 XML文件）、冲突很多或者两个版本很不一致时，试着用下面的命令再次合并：
```js
git merge --strategy-option=patience
```

##### 4. 当你需要单个文件的历史信息时
除非使用像 SourceTree 一样的可视化工具来弄清到底对一个文件做过什么，不然你可以使用：
```js
//这个没搞明白
git log --merge --decorate --source -p path/to/file/you/care/about
```


### 手动解决冲突

处理合并问题主要有两类做法：有些开发者喜欢偏底层处理，因而手工操作处理冲突标记，而有些则偏好可视化工具来辅助（解决冲突）。两种方式都能极其有效地解决冲突。

##### 5.处理过程示例
有几个同事分享了他们手动处理的过程，比如詹森·欣奇描述了他的处理流程：
```js
git merge the/other/branch
git status
```

- 看下有多少文件冲突。
- 对每个冲突文件：
- 1.在编辑器中打开文件（比如 IntelliJ 或 Vim ）
- 2.看看每个被冲突标记（“>>>>”和“<<<<”）围绕的区块。
- 3.看看（被冲突标记的区块）是否有意义，每个作者的意图是什么，如果能弄清楚就解决掉。
- 4.如果冲突标记无法理解，通常是这些文件改动很大，运行下面的命令：
```js
git diff HEAD...the/other/branch -- path/to/conflicting/file
git diff the/other/branch...HEAD -- path/to/conflicting/file
```
这样做是为了看哪边改动较小
- 通常下面的命令：
```js
git log -p HEAD..the/other/branch -- path/to/conflicting/file
git log -p the/other/branch..HEAD -- path/to/conflicting/file
```
能帮助理解另一边改动了什么。
- 回溯文件到改动最大的一边：
```js
git checkout the/other/branch -- path/to/conflicting/file
```
- 手动检查并且再重新应用从另一边对文件的更改:
```js
git add path/to/conflicting/file
```
- 当这些更改都修复之后要构建整个项目，确保至少可以编译通过，如果测试可以很快运行起来，也要运行一下这些测试：
```js
git commit
```
这个过程看起来有点太手工化了，但詹森发现对于他的工作流程来说会更少产生不合理的合并。    
想看一步步手动解决冲突的基本视频，可以参看我们最近的 Git Power Routines的
[教程](https://www.youtube.com/playlist?list=PLDshL1Z581YYxLsjYwM25HkIYrymXb7H_)


### 合并工具的天堂   
有很多不同的可视化工具来操作复杂的合并和解决冲突。我的同事们提到了一些（并不是所有）：

##### 6. IntelliJ IDEA 冲突解决工具：  
IntelliJ IDEA 是很多 Atlassian 工作人员使用的 IDE 。很多人使用它内建的冲突解决工具来处理冲突，它提供了三个面板来分析：本地、远程和合并结果。

##### 7. KDiff3
KDiff3 是 KDE 产品系列的一部分，并且在我们内部调查时提到过几次。

##### 8. P4Merge
SourceTree 的作者斯蒂夫·斯特里廷和其他几个同事使用 P4Merge 来执行合并操作。P4Merge 是免费的可视化工具，它具有四个面板而不是其他工具提供的三个，显示了”base“、”local“、”remote“和”merge result“。

##### 9. meld
meld 是用 GTK+ 和 Python 开发的，也是已经存在了很久的工具，被提到了几次。

##### 10. tig for status + diff
更多喜欢终端的人使用 tig （我们之前写过 tig 的一篇介绍），再加上 “ git diff ”。

##### 11. OS X 下的 FileMerge 即 opendiff
在长长的建议列表里，有几个开发者提到了 OS X 下原生的“opendiff”工具，或者叫做“FileMerge”。

##### 12. diffmerge
我并不知道 diffmerge 这个工具，不过在列表里也被提到了。

##### 13. Araxis Merge (商业版)
Araxis Merge 这个名字让我想起了很久以前。那时，我在一台 Windows 机器上一堆让人抓狂的 XML 文件中垂死挣扎，而这个工具证明了它可以经受住这个挑战。它是个商业软件。

##### 14. vimdiff3
有几个同事用 vimdiff 来解决冲突，那是 vim 自带的合并/差异分析 工具，你可以这样设置：      

```js
git config merge.tool vimdiff
git config merge.conflictstyle diff3
git config mergetool.prompt false
```

或者按照上面展示的那样直接修改 .gitconfig 文件。
