---
layout: post
title:  "drbd脑裂的一般处理"
date:   2017-01-22 15:05:00 +0800
categories: "debug"
tag: ["python", "linux"]
---


* content
{:toc}

dns日志

    no address range available for DHCP request via eth3

网上搜索回复都是子网设置问题,看了半天配置文件没看出问题,配置文件改来改去也没发解决,最后只能硬着头皮读dnsmasq的源码然后调试了
这个是我第一个文件，只是测试用的，好了再说
