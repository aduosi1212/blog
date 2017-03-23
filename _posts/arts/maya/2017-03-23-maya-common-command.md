---
layout: post
title:  "maya的命令介绍"
date:   2017-03-23 09:35:00 +0800
categories: "maya"
tag: ["command"]
---

* content
{:toc}

### 1. 取线：
  Nurbs和Plygon都可以取线，对象转到边的操作模式，选择要取成线的边，然后Curves->Duplicate Surface Curves，就可以把线取出来了，如果是Polygon的话，这个方法取出的线就是一段一段，Polygon要取连续的线的方法，Modify->Convert->Polygon Edge to Curves就可以了。

### 2. Attach 和 Detach:
- 1.  如果是Curves->Attack，就是两根线连到一起， 每根线都可以设置起始连接点，选中线右键Curve Point选取连接点， 第二根线shift加选连接点就可以了；Detach就是根据CurvePoint断开，和Attach相反。
- 2.  如果是Surface->Attack，就是两个面连到一起，面可以设置连接边， 右键isoparm选择一个面的，Shift加选第二个面的边，决定哪两个面连接；Detach根据iosparm选择的线把面切开。  
*ps. 调整正反面， Surface->Reverse Direction。*

### 3. Align:
- 1. 只是是一个对齐关系，不会合并到一起。

### 3. Open/Close:
- 1. 不管是线还是对象，都是对应打开/闭合，如果是对象，注意设置里面有UV方向控制方向。
