---
layout: post
title:  "Git的配置文件"
date:   2017-09-26 15:05:00 +0800
categories: "git"
tag: ["config"]
---


* content
{:toc}

Win10的全局配置目录：C:\Users\Administrator\.gitconfig, 这个里面可以设置一个全局可用信息，比如用户名之类，除了在这里直接打开之外，还能通过命令打开：git config [--global] --edit。

##### 如果要设置全局用户名，操作如下：
```js
git config --global user.name "youName"
```

Win10 局部配置，git工程设置不同的配置文件，那么git config就在该工程的 .git/config文件中

##### 如果要设置全局用户名，操作如下：
```js
git config user.name "youName"
git config user.email youemialName@example.com
```
