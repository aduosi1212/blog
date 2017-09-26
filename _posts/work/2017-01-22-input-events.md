---
layout: post
title:  "title"
date:   2017-01-22 15:05:00 +0800
categories: "工作"
tag: ["input","events"]
---


* content
{:toc}

###### 1. 虚幻4各个平台的输入事件分发的开始点位置，LaunchEngineLoop.cpp文件中：
```c++
void FEngineLoop::Tick()
{
      //此处省略n行
      if (FSlateApplication::IsInitialized() && !bIdleMode)
      {
            SCOPE_TIME_GUARD(TEXT("SlateInput"));
            QUICK_SCOPE_CYCLE_COUNTER(STAT_FEngineLoop_Tick_SlateInput);

            FSlateApplication& SlateApp = FSlateApplication::Get();
            SlateApp.PollGameDeviceState();
            // Gives widgets a chance to process any accumulated input
            SlateApp.FinishedInputThisFrame();
      }
      //此处省略n行
}
```

Android输入事件存放在：FAndroidInputInterface::TouchInputStack。
