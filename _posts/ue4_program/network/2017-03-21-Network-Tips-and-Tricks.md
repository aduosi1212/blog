---
layout: post
title:  "虚幻4的属性网络复制机制"
date:   2017-03-21 11:05:00 +0800
categories: "ue4"
tag: ["newwork"]
---


* content
{:toc}

基础属性的复制(Basic Property Replication), 主要讲一下属性复制在代码里面怎么操作，要实现网络属性复制要做以下几件事：
- 1. 在 Actor Class 里面定义属性，要确保UPROPERTY声明里有replicated关键字。
```c++
class ENGINE_API AActor : public UObject
{
UPROPERTY( replicated )
AActor * Owner;
};
```

- 2. 在类实现里面，要重写GetLifetimeReplicatedProps函数并实现它。
```c++
void AActor::GetLifetimeReplicatedProps( TArray< FLifetimeProperty > & OutLifetimeProps ) const
{
DOREPLIFETIME( AActor, Owner );
}
```

- 3. 在类的构造函数里面，确保属性bReplicates设置为true。
```c++
AActor::AActor( const class FPostConstructInitializeProperties & PCIP ) : Super( PCIP )
{
bReplicates = true;
}
```
这样类成员属性Owner将会同步复制当前实例到所有当前连接到它的客户端。

下面再说下条件复制, 一旦一个属性被注册为可复制，就不能取消注册（这就是生命周期的由来？？？），这样做的原因是尽可能拷贝更多的信息，这样能在把同一组属性的信息同步给更多的网络链接，节省更多的计算时间，明白。。。。。。
所以要控制属性的复制只能通过条件，默认的，每一个可复制的属性都有一个内置的条件，就是如果它不改变就不会复制。为了了得到更多的复制控制权，还定义了一系列宏给予一个第二条件去控制，宏的名字为DOREPLIFETIME_CONDITION。使用例子如下：
```c++
void AActor::GetLifetimeReplicatedProps( TArray< FLifetimeProperty > & OutLifetimeProps ) const
{
DOREPLIFETIME_CONDITION( AActor, ReplicatedMovement, COND_SimulatedOnly );
}
```
COND_SimulatedOnly标记放入条件宏里会对复制执行额外的检测。在上面的例子中只有产生一个模拟复制才回把属性复制到客户端（这里不是很懂，后面用再实验下吧）。
这样最大的好处就是节省宽带（废话），

一下为当前支持的条件检测：
- COND_InitialOnly - This property will only attempt to send on the initial bunch
-	COND_OwnerOnly - This property will only send to the actor's owner
-	COND_SkipOwner - This property send to every connection EXCEPT the owner
-	COND_SimulatedOnly - This property will only send to simulated actors
-	COND_AutonomousOnly - This property will only send to autonomous actors
-	COND_SimulatedOrPhysics	- This property will send to simulated OR bRepPhysics actors
-	COND_InitialOrOwner - This property will send on the initial packet, or to the actors owner
-	COND_Custom - This property has no particular condition, but wants the ability to toggle on/off via SetCustomIsActiveOverride

条件复制可以对引擎更容易进行优化并且给予了足够的控制权。

如果觉得控制权还不够，下面还有一个主题，还有一个宏叫DOREPLIFETIME_ACTIVE_OVERRIDE，可以给予你足够的控制权决定一个属性要不要复制，可以用任意的通用条件判断，这有一个说明就是这针对的是每个actor，而不是每个链接，换句话说就是用一个通用条件去控制某个链接该不该复制是不安全的，例子如下：
```c++
void AActor::PreReplication( IRepChangedPropertyTracker & ChangedPropertyTracker )
{
DOREPLIFETIME_ACTIVE_OVERRIDE( AActor, ReplicatedMovement, bReplicateMovement );
}
```
以上代码只要bReplicateMovement为true，属性ReplicatedMovement就会被复制到客户端。
为什么不直接用这个去判断所有的复制，下面两个原因：
- 1.如果频繁改变复制条件，就会让速度慢下来。
- 2.这个条件不能改变每个链接，因为他是针对actor的。

每个复制条件都给予一个有效的平衡在控制和性能上，他们给了引擎时机优化了时间去检测和发送属性到很多链接,同时给了开发者很好的控制权去决定什么时候怎样进行属性同步。
