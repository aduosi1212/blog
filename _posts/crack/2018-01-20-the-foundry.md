---
layout: post
title:  "The Foundry Products"
date:   2018-01-20 11:05:00 +0800
categories: "The Foundry公司产品破解"
tag: ["crack"]
---


* content
{:toc}

- 1.安装产品Nuke11。

- 2.安装 FLT7服务。

- 3.停止证书许可证服务器(从Foundry License Utility停止)。

- 4.复制破解rlm.foundry.exe到(C:\Program Files\The Foundry\LicensingTools7.1\bin\RLM\rlm.foundry.exe)。

- 5.编辑xf_foundry.lic取代host_name mac_address端口(mac_address中间不要有-)。你可以使用rlmutil.exe获得这些信息，一个简单的DOS窗口ipconfig /all或者你可以开始安装许可证实用程序(Foundry License Utility)，选择诊断和运行。

诊断，你会发现你的主机，你的MAC地址（ID）端口是默认的5053，如果您不想指定一个端口，请不要忘记删除。

- 6.复制xf_foundry.lic到C:\ProgramData\The Foundry\RLM or C:\Program Files\The Foundry\LicensingTools7.1\bin\RLM\

- 7.重启服务器可以从一个服务或只是安装许可效用（检查日志和信息的最佳方式）选择启动服务器的RLM服务器并单击

- 8.启动应用(NukeX、Mari之类), Licence报错，选择从服务器安装，服务器地址(5053@127.0.0.1)安装.

- 8.破解完成。