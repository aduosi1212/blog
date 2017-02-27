---
layout: post
title:  "Markdown的一些语法"
date:   2017-01-22 15:05:00 +0800
categories: "Markdown"
tag: ["syntax"]
---


* content
{:toc}

标题：
# h1级标题
## h2级标题
### h3级标题
#### h4级标题
##### h5级标题
###### h6级标题
----
分割线：三个以上的短线 即可作出分割线

----

[百度](https://www.baidu.com/)

[<i class="icon-refresh"></i> 点我刷新](/sonfilename/)

另一种超链接写法：[链接名][链接代号]
[here][3]
然后在别的地方定义 3 这个详细链接信息。
[3]: http://www.izhangbo.cn "聚牛团队"

直接展示链接的写法：<http://www.izhangbo.cn>

键盘键
<kbd>Ctrl+[</kbd> and <kbd>Ctrl+]</kbd>

code格式：反引号
Use the `printf()` function.

``There is a literal backtick (`) here.针对在代码区段内插入反引号的情况``

强调：
*斜体强调*
**粗体强调**

图片
![Alt text](https://timgsa.baidu.com/timg?image&amp;quality=80&amp;size=b9999_10000&amp;sec=1488193484199&amp;di=ab82c71c1038356ad045b0433320affc&amp;imgtype=0&amp;src=http%3A%2F%2Fimg.zcool.cn%2Fcommunity%2F01e66b572321bc32f875a3992a6871.jpg "图片提示")

使用 icon 图标文字
<i class="icon-cog"></i>

段落：以一个空行开始，以一个空行结束，中间的就是一个段落。

这是一个段落

表格：

Item     | Value
-------- | ---
Computer | $1600
Phone    | $12
Pipe     | $1

无序列表：使用 - 加一个空格（）

- 无需列表1
- 无序列表2
- 无序列表3

有序列表：使用 数字 加一个英文句点

1. 有序列表
2. 有序列表
3. 有序列表
4. 有序列表
5. 有序列表

换行缩进形成代码区块

    这里先换行，然后缩进4个空格，之后的内容便可以原样显示了，适合用于显示代码内容。直到文本结束或最后一个存在缩进的行为止。

块引用
>给引用的文本开始位置都加一个 '>'，

>便可组成一个块引用。在块引用中，可以结合

>其他markdown元素一块使用，比如列表。

>**强调**

也可以只在第一行加大于号，其他位置不加。

>- 块引用里使用列表，需要和上面的内容隔开一个空行
>- 记得加空格哦。


视频
<video id="video" controls="" preload="none" poster="http://media.w3.org/2010/05/sintel/poster.png">
    <source id="mp4" src="http://media.w3.org/2010/05/sintel/trailer.mp4" type="video/mp4">
    <source id="webm" src="http://media.w3.org/2010/05/sintel/trailer.webm" type="video/webm">
    <source id="ogv" src="http://media.w3.org/2010/05/sintel/trailer.ogv" type="video/ogg">
    <p>Your user agent does not support the HTML5 Video element.</p>
</video>
