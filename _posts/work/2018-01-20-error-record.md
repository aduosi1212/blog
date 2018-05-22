---
layout: post
title:  "ue4 error record"
date:   2018-01-20 15:05:00 +0800
categories: "工作"
tag: ["error","record"]
---


* content
{:toc}

### 1. VS2015 编译引擎时可能会报虚拟内存越界的问题(PCH，/ZM)

解决：1. C:\Users\username\AppData\Roaming\Unreal Engine\UnrealBuildTool\BuildConfiguration.xml 文件修改为：
如果bDisableDebugInfo 和 bOmitPCDebugInfoInDevelopment为true的话没有调试信息，开发版本的话还是改成false
```c++

<Configuration xmlns="https://www.unrealengine.com/BuildConfiguration">
    <BuildConfiguration>
        <ProcessorCountMultiplier>1</ProcessorCountMultiplier>
        <bDisableDebugInfo>true</bDisableDebugInfo>
        <bOmitPCDebugInfoInDevelopment>true</bOmitPCDebugInfoInDevelopment>
        <bUseSharedPCHs>false</bUseSharedPCHs>
   </BuildConfiguration>
</Configuration>

```

解决：2. VCToolChain.cs 文件中的/Zm***修改为/Zm400，值如果不对可以多尝试几个.

解决：3. 在ModuleRules继承类中加入 PCHUsage = PCHUsageMode.UseExplicitOrSharedPCHs，PCHUsageMode是个枚举，有3个类型：
```c++

public enum PCHUsageMode
{
/// <summary>
/// Default: Engine modules use shared PCHs, game modules do not
/// </summary>
Default,

/// <summary>
/// Never use shared PCHs. Always generate a unique PCH for this module if appropriate
/// </summary>
NoSharedPCHs,

/// <summary>
/// Shared PCHs are OK!
/// </summary>
UseSharedPCHs,

/// <summary>
/// Shared PCHs may be used if an explicit private PCH is not set through PrivatePCHHeaderFile. In either case, none of the source files manually include a module PCH, and should include a matching header instead.
/// </summary>
UseExplicitOrSharedPCHs,
}

```


### 2. 编译时可能会出现警告warning C4828: 文件包含在偏移 0x377 处开始的字符，该字符在当前源字符集中无效(代码页 65001)。   
这是编码问题，可以把文件修改为Unicode(UTF-8 无签名) 就可以去掉警告


### 3. 编译时可能会出现错误cpp文件必须包含对应的头文件为第一个，原因是使用了 PCHUsage = PCHUsageMode.UseExplicitOrSharedPCHs的编译方式。



