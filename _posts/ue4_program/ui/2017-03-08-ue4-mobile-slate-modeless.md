---
layout: post
title:  "虚幻4中移动平台ui以弹非模态形式弹出"
date:   2017-03-09 10:05:00 +0800
categories: "ue4"
tag: ["moblie", "slate"]
---
* content
{:toc}

ue4中的SWidget如果是以AddViewportWidgetContent的方式添加到Viewport中的话都会变成模态的的方式添加，但是有时候我们会希望弹出框不响应鼠标或者touch事件，并且不会遮住底部ui的响应。  

SWidget的源码中有一个接口IsInteractable，这个接口在FHittestGrid类中用到，FHittestGrid类捕获点击SWidget时用到了FGridTestingParams结构，FGridTestingParams结构有一个变量bTestWidgetIsInteractive，这个变量如果为true的话，就会判断SWidget是否IsInteractable可响应，如果IsInteractable返回false的话，这个SWidget就会被忽略点击事件。

这样就可以实现弹出SWidget不会响应事件，而且不会遮挡底部的ui响应。

但是bTestWidgetIsInteractive这个变量默认为false，如果强制改为true的话，会使引擎编辑器部分ui也不响应，所以只有判断在打包成游戏模式时改为true，并且游戏中要响应的ui的IsInteractable这个必须返回true。

*暂时只能找到这个方法，如果以后有更好的方法再说*
