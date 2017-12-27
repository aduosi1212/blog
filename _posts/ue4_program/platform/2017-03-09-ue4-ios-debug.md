---
layout: post
title:  "ue4中ios的调试"
date:   2017-03-08 11:05:00 +0800
categories: "ue4"
tag: ["debug", "ios"]
---

* content
{:toc}

Code Signing Entitlements

授权机制。在Xcode的capabilities选项卡下选择一些选项后，Xcode就会生成这样一段XML，Xcode会自动生成一个entitlements文件，然后再需要的时候往里面添加条目。当构建整个应用时，这个文件也会提及给codesign作为应用所需要拥有哪些授权的参考。这些授权信息必须都在开发者中心的AppID中启用，并且包含在配置文件中。

Code Signing Identity

配置证书

Development Team

开发者所在的群组

Other Code Signing Flags

常见的配置为--deep。用法不详。

Provisioning Profile

配置描述文件。
