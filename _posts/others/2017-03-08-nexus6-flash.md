---
layout: post
title:  "nexus6刷机的一些问题"
date:   2017-03-08 15:05:00 +0800
categories: "android"
tag: ["flash"]
---


* content
{:toc}

nexus6用久了，动不动就卡得跟狗一样，想刷一些干净些的系统，每次刷机完了过一段时间又忘了，折腾起来很麻烦。
-  刷机步骤：  
##### 1. 开机按住声音下键（就是减音量的按键）的同是按住开机键（电源键），系统会进入fastboot模式。
##### 2. 用音量上下键选择到recovery mode，按开机键进入该模式，这时可能遇到一个问题就是出现叹号，并且屏幕中出现no command的字样，出现这个问题时按住开机键几秒钟后按一下声音上键，才会真正进入recovery mode。
##### 3. 双清系统，选择wiping cache, 清除用户数据，这个要等一会。选择wiping data，还原系统，这个要等n久。
*这种方法我没双清成功，wiping data这里卡住了，所以清理数据还是强烈建议装第三方的recovery，例如Nexus6_TWRP，直接进入选择wiping，这样就可以双清了。*
##### 4. 双清完就装系统，第三方TWRP中点安装(instal)按钮，选择sd卡中的系统包，执行安装。

- 第二种刷机方式：
这个方法主要针对官方镜像，只要开了oem锁，无论怎样，用这样方法都能刷回来。  
*貌似这种方法也要先能进入recovery，233...，如果recovery都进不去还没试过......*
##### 1.去下一个官方的镜像包（类似shamu-n6f26u-factory-6e52aa95.zip这种），解压后会有一些批处理文件和另一个zip包，这个zip包含了所有要用到的img镜像。
##### 2.然后需要用到adb和fastboot工具。
##### 3.加压所有的img文件拷贝到fastboot.exe同一个目录（方便处理），开机让手机进入recovery模式（音量下键+开机键），然后用以下命令一个一个刷进去就行了：
```js
fastboot flash recovery recovery.img
fastboot flash boot boot.img
fastboot flash system system.img
fastboot flash cache cache.img
fastboot flash userdata userdata.img
fastboot reboot
```
或者修改下fastboot-all.bat批处理执行：
```js
@ECHO OFF  
:: Copyright 2012 The Android Open Source Project  
::  
:: Licensed under the Apache License, Version 2.0 (the "License");  
:: you may not use this file except in compliance with the License.  
:: You may obtain a copy of the License at  
::  
::      http://www.apache.org/licenses/LICENSE-2.0  
::  
:: Unless required by applicable law or agreed to in writing, software  
:: distributed under the License is distributed on an "AS IS" BASIS,  
:: WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.  
:: See the License for the specific language governing permissions and  
:: limitations under the License.  
PATH=%PATH%;"%SYSTEMROOT%\System32"  
fastboot flash bootloader bootloader-shamu-moto-apq8084-71.08.img  
fastboot reboot-bootloader  
ping -n 5 127.0.0.1 >nul  
fastboot flash radio radio-shamu-d4.0-9625-02.98.img  
fastboot reboot-bootloader  
ping -n 5 127.0.0.1 >nul  
::fastboot -w update image-shamu-lmy47e.zip  
fastboot flash recovery recovery.img  
fastboot flash boot boot.img  
fastboot flash system system.img  
fastboot flash cache cache.img  
fastboot flash userdata userdata.img  
echo Press any key to exit...  
pause >nul  
exit  
```
*第三方recovery装不了的话也可以用第二种方法先装一个干净的系统再装第三方recovery*
