---
layout: post
title:  "虚幻4易接sdk接入"
date:   2017-05-23 10:05:00 +0800
categories: "ue4"
tag: ["android", "sdk"]
---
* content
{:toc}

易接sdk接入关键点：

- 1.在application总增加一个属性android:name，创建一个它的application, 名称为com.snowfish.cn.ganga.offline.helper.SFOfflineApplication
```js
<application android:label="@string/app_name" android:icon="@drawable/icon" android:hasCode="true" android:name="com.snowfish.cn.ganga.offline.helper.SFOfflineApplication" android:allowBackup="true">
```


- 2.要把虚幻4的SplashActivity换成易接sdk它自己的SFGameSplashActivity。
```js
<activity android:name="com.snowfish.cn.ganga.offline.helper.SFGameSplashActivity" android:label="@string/app_name" android:theme="@style/UE4SplashTheme" android:launchMode="singleTask" android:screenOrientation="landscape" android:debuggable="true">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
</activity>
```

intent-filter块定义了Android应用的默认启动Activity。

- 3.UE4中Android接收的输入接口 HandleInputCB，sdk中要调用android的返回按钮的话，就只能用这个，原生Activity的OnKeyDown无效



 遇到的各种问题记录：

- 1.在启动的时候崩溃

```c++
java.lang.RuntimeException: Unable to start activity ComponentInfo{com.YourCompany.KingWorld/com.snowfish.cn.ganga.offline.helper.SFGameSplashActivity}: android.content.res.Resources$NotFoundException: String resource ID #0x0
```
原因和解决：因为SFGameSplashActivity在OnCreate初始化阶段就会找你游戏的主Acitivity名字路径， 该名字定义在res/values/strings.xml中的sf_class_name字段中，如果没有找到就会崩溃。
