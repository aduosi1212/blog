---
layout: post
title:  "ue4中android的调试"
date:   2017-03-08 11:05:00 +0800
categories: "ue4"
tag: ["debug", "android"]
---


* content
{:toc}

##### 2. 问题
- 1. Nsight Tegra 装好，运行调试部署启动应用都成功，但是gdbserver启动失败，出现以下错误：
```js
"Cannot launch gdbserver: all options have failed."
"Failed to attch: Failed to start debug server on the device"
```  

原因：can't create /data/data/com.YourCompany.appname/gdbserver': Permission denied". gdbserver没有启动权限，没有root的设备，要使用gdbserver 调试app 会遇到权限问题。

解决：一种最简单的方案：升级系统，高级系统可能解决了这个问题，我的nexus6 最新 7.1系统就没问题，可以正常测试.

第二种：编辑AndroidManifest.xml文件，在application中加入属性android:debuggable="true"
```js
<application android:label="@string/app_name"
android:icon="@drawable/icon"
android:hasCode="true"
android:name="com.snowfish.cn.ganga.helper.SFOnlineApplication"
android:allowBackup="true"
android:debuggable="true">
```
一般手机都可以调试了，但三星不行，不知道为啥。

第三种: Modify you build scripts to include gdbserver in your "*.apk/libs.so" gdbserver would be intalled with you apk。第二种还不知道怎么弄。
