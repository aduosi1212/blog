---
layout: post
title:  "maya的安装错误记录"
date:   2017-03-22 10:05:00 +0800
categories: "arts"
tag: ["maya", "errors"]
---

* content
{:toc}

#### 1. could not open key unknown components:
读取不到注册表Components的权限，路径HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Installer\UserData\S-1-5-18\Components

- 1. 打开REGEDIT注册表
- 2. 知道到以上路径位置
- 3. 右键选择权限
- 4. 点击高级按钮
- 5. 在所有者那一行选择更改
- 6. 更改所有者到 Administrators (组).
- 6. Then select the Replace owner on subcontainers and objects check box and click Apply.
- 7. Go to the Permissions tab, select the check box Replace all child objects Permissions with inheritable permissions from this object, and click apply. Note that the wording maybe a slightly different depending on your operating system.

#### 2. autodesk软件安装源文件里面都有一个辅助卸载工具，如果出什么问题可以用他卸载
