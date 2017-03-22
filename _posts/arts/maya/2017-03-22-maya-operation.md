---
layout: post
title:  "maya的一些操作"
date:   2017-03-22 10:05:00 +0800
categories: "arts"
tag: ["maya"]
---

* content
{:toc}

##### 1. 界面布局：
- I. 语言切换，在maya的安装目录找到文件夹resources/l10n/, 里面又一个子文件夹zh_CN，这个名字如果是zh_CN就是中文，如果改成en_US就是英文， 或者在环境变量中新建变量MAYA_UI_LANGUAGE，变量值zh_CN和en_US就代表中英文。
- II. maya 界面区域菜单控制全部在window里面， Ctrl+m , Shift+m, Alt+Shift+m可以关闭打开几个子视图上边的菜单， 如果想重置maya菜单，可以把我的文档->maya文件夹删掉。
- III

##### 1. 物体移动操作：  
鼠标中键和shift组合可以移动物体；  
磁铁工具可以快速移动物体或者点线面；    
在ChanelBox中选中属性名字，鼠标中键在工作区拖动（属性中可以做+-x/的操作，例如 += 5.2， /= 2.2）；    
在Attribut Editor中输入框选中，Ctrl+鼠标左键拖动时移动小数点第2位，Ctrl+鼠标中键拖动时移动小数点第1位，Ctrl+鼠标右键拖动时移动整数位；  
Insert（或者按住D）键可以调整物体中心点， Modify->CenterPrivot可以重置中心点

##### 2. 物体选中：
  1. Ctrl 减选；
  2. Shift 加选或者反选；
  3. Ctrl+Shift 叠选；
  3. 套索和笔刷选取;
  4. Outliner中选择和 Windows 中的选中操作一样。

##### 3. 零件和对象模式切换：
  1. 右键菜单可以选择，返回对象模式时可以右键选择Select返回并选中;
  2. 菜单图示选择;  
  3. F8, 零件固件切换(没有特定选择类型)， Polygon物体可以用F9， F10， F11对应点线面切换。 *(ps.F12为uv)*

##### 4. 对象的显示隐藏：   
  1. 右边的Channel Box属性Vsibility的on/off控制对象的显示隐藏（可以输入1/0代替）， 快捷键，选中Ctrl+H隐藏，Outliner选中Shit+H显示， Ctrl+Shift+H显示最后隐藏的对象。

##### 5. 左边独立选项使用：
  1. 柔化选取, 三个选项都有柔化选取，物体切换为零件模式， 柔化打勾， 就可以实现柔化选取，在场景中按b可开关。按住b，鼠标左键拖动可调整柔化范围。
  2. Common selection options, 框选（Marquee），拖选（Drag）。
  3. Symmetry Setting, 对称选取， 可根据三个坐标轴做对称选取(有世界坐标和对象坐标)。
  4. 移动选项中的Move Settings 有一个Axis Orientation可以设置根据哪一个轴的方向移动（只在零件模式下有效），比如法线等等；
  5. 缩放选项里有一个Prevent Nagative Scale防止反过来做缩放；
  6. 移动设置里又一个Move Snap Settings, 打钩去掉后配合x使用，可使点对齐；
  7. 移动设置里有一个Joint orient Seetings 可以使骨骼的joint跟随骨架转向；
  8. 工作区按住J，可进行固定步进移动，对应各个设置里面的Step Snap。
*ps 工作区按住对应快捷键，同时按住鼠标左键，可快速打开菜单*

##### 6. 建模完成后参数归0：
  1. Modify->Freeze Transformations；
  2. 可以在属性面板右键Freeze->, 然后可以选择归0的项；
  3. Edit菜单， Edit->Delete删除历史。
  4. Modify->Reset Transformations, 可以中心点归到原点位置，首先要Freeze对象。

##### 7. 自定义shelf，就是工作区上面那一排图形快速操作按钮：
  1. 选择最左边的齿轮，New Shelf，起一个自己的名字；
  2. 找到自己想要的指令，Ctrl+Shift+鼠标左键，就会添加到Shelf里面；
  3. 右键选择Shelf里面的命令，在打开的菜单里选择Open，就会打开Shilf Editer，里面可以编辑信息。
  4. 组合指令，把指令添加到Shelf后如果要组合使用，把要组合的指令右键Edit，在Command里把命令复制到另一个父命令的Command里面，像编程一样用分号做命令的结束符：
  ```js
  FreezeTransformations;
  DeleteHistory;
  ```
  5. 自定义命令，右下角Script Editer, 选中所有命令文本，鼠标中键拖到Shelf里面，可以做一些简单的编辑就可以直接使用（这个牛逼），一些做好的模型可以直接记录下来，下次可以直接使用。

##### 8. 自定义hotbox和hotkey：
  1. Windows->Settings/Preferenes->Marking Menus Editer打开hotbox编辑器, Create Marking Menu新建一个Menu面板；
  2. 鼠标中键拖动Shilf中的命令到面板的各自里，各自右键可以编辑命令信息，可以用中文，还可以在相应的格子中新建一个子菜单，建完后起一个名字；
  3. 在Use marking menu in的下拉菜单选择一个使用方法，有Hotbox和Hotkey editer；
  4. 先说Hotbox，然后在出现的5个位置中选择一个，鼠标响应方式选一个， 就可以在主hotbox弹出后在相应的方向点击对应的鼠标响应方式就可以使用；
  5. 然后是hotkey，紧接第3步，选择Hotkey Editer，关闭，在Windows->Settings/Preferenes->Hotkey Editer, ，在Hotkey Editer面板的Edit Hotkeys For下拉菜单中选择Other Items，在出现的列表中选择User Marking Menus就会出现刚才定义的菜单，有Press和Release两个定义，点击在出现的输入框里输入你想要的hotkey，注意下面的提示，不要覆盖原先的hotkey，输入完成后保存，在工作区间按你刚指定的hotkey后鼠标左键点击就出现菜单了。

##### 9. 曲线绘制基础
  1. 基本曲线，Create->Curve Tools可以绘制曲线，曲线在maya2017中有6种，除了bezier曲线外，其他标记在outliner中都一样，ep 曲线比较好用，曲线绘制错了，可以用Del或者Backspace回退删除，Esc删除整个曲线，过程中可以按Ins键进行调整，再按Ins键取消调整接着绘制，绘制完成后Enter结束；
  2. 文字曲线Create->Type，可以生成文字曲线，在右边属性编辑栏可以编辑内容和信息；
  3. Create->Adobe(R)Illustrator(R)Object...，这个用到ps的转存功能，在ps转存曲线路径为Illustrator，一个.ai文件，然后打开为曲线，可以使平面或者立体方式，ps中的paths是什么形状，创建出来的曲线就是什么形状；
  *举个例子： 新建一个ps文档，用曲线选择工具选出一个轮廓区域，在右边paths（图层旁边）工作区底部菜单转存为路径，然后File->Export->paths to Illustrator转存为一个ai文件，再在maya中创建曲线*
  4. 立体曲线功能，可以自动生成。
  5. 曲线编辑，上下左右键可以挨个选择曲线的点，可以理顺先后关系（比如打结）。
