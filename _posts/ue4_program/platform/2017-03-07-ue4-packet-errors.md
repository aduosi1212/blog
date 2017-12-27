---
layout: post
title:  "虚幻4打包的一些错误"
date:   2017-03-07 15:05:00 +0800
categories: "ue4"
tag: ["android", "packet"]
---


* content
{:toc}

#### 1. Android

记录虚幻4中android打包所遇到的一些错误和问题。
- 1  android sdk 环境变量错误
```js
UATHelper: 打包 (Android (ETC1)): UnrealBuildTool: The "android" command is no longer available.
UATHelper: 打包 (Android (ETC1)): UnrealBuildTool: For manual SDK and AVD management, please use Android Studio.
UATHelper: 打包 (Android (ETC1)): UnrealBuildTool: For command-line tools, use
UATHelper: 打包 (Android (ETC1)): UnrealBuildTool: tools\bin\sdkmanager.bat and tools\bin\avdmanager.bat
UATHelper: 打包 (Android (ETC1)): UnrealBuildTool: ERROR: D:/NVPACK/android-sdk-windows/tools/android.bat failed with args --silent update lib-project --path JavaLibs/downloader_library --target android-19
```
之前用ue4自带的CodeWorksforAndroid安装了android的sdk，后面装了android studio之后把android-sdk-windows/tools/android.bat篡改了。你妹的android studio不允许命令启动sdk和avd管理程序，必须从android studio启动。


- 2 JNI DETECTED ERROR IN APPLICATION: GetStringUTFChars received NULL jstring会引起程序崩溃：  
Jni 字符转换GetStringUTFChars()函数的传入参数不能是NULL.

- 3 应用名显示不出来：
原因还没找到，更新旧的string.xml就好了

#### 1. IOS
Failed to run init commands on 192.168.1.***. Output = ***   
Failed to initialize a connection to the Remote Server 192.168.1.***

解决方法1：bUseRSync=False 改为False，不用ssh协议   
解决方法2：http://blog.unreal-mobile.com/remote-build-on-ios-windows-10-to-xcode-9-arkit-project/ 这篇文章，还没试过

