---
layout: post
title:  "maya的一个ue4骨骼绑定插件ArtV1的使用"
date:   2017-05-12 12:05:00 +0800
categories: "arts"
tag: ["maya", "ue4", "plugin"]
---

* content
{:toc}

### 1. 安装：  
- 1. 到官方Epic Game启动程序商店中下载ArtV1，安装后在“Epic Games\UE_4.15\Engine\Plugins\Marketplace\ARTv1” 目录中可以看到，为了方便管理，可以把ARTV1文件夹内容拷贝一个其他地方。
- 2. 把MayaTools目录中的userSetup.py拷贝到文档->maya->scripts目录下，启动maya。
- 3. 启动maya后会弹出一个Maya Tools Install窗口，点击窗口中的Browse浏览到MayaTools目录，然后点击确定，安装完成，成功后会在maya的菜单栏中看到一个"A.R.T.1.0"的菜单。

### 2. 更新：
更新很简单，在Epic Game启动程序中安装了该插件以后，如果有更新，在引擎启动下面点击已安装的插件，在弹出框中选择Update，更新完成后到“Epic Games\UE_4.15\Engine\Plugins\Marketplace\ARTv1”目录下拷贝MayaTools覆盖到之前的存放点就ok了。
