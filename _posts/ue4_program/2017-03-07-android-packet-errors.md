---
layout: post
title:  "虚幻4android打包的一些错误"
date:   2017-03-07 15:05:00 +0800
categories: "ue4"
tag: ["android", "packet"]
---


* content
{:toc}

记录虚幻4中android打包所遇到的一些错误和问题。
- 1. android sdk 环境变量错误
```js
UATHelper: 打包 (Android (ETC1)): UnrealBuildTool: The "android" command is no longer available.
UATHelper: 打包 (Android (ETC1)): UnrealBuildTool: For manual SDK and AVD management, please use Android Studio.
UATHelper: 打包 (Android (ETC1)): UnrealBuildTool: For command-line tools, use
UATHelper: 打包 (Android (ETC1)): UnrealBuildTool: tools\bin\sdkmanager.bat and tools\bin\avdmanager.bat
UATHelper: 打包 (Android (ETC1)): UnrealBuildTool: ERROR: D:/NVPACK/android-sdk-windows/tools/android.bat failed with args --silent update lib-project --path JavaLibs/downloader_library --target android-19
```
之前用ue4自带的CodeWorksforAndroid安装了android的sdk，后面装了android studio之后把android-sdk-windows/tools/android.bat篡改了。你妹的android studio不允许命令启动sdk和avd管理程序，必须从android studio启动。
