---
layout: post
title:  "title"
date:   2017-01-22 15:05:00 +0800
categories: "工作"
tag: ["input","keyboard"]
---


* content
{:toc}

###### 1. Android平台密码打开输入框引发崩溃问题
解决：AndroidThunkJava_ShowVirtualKeyboardInputDialog函数中修改为：
```c++
//	virtualKeyboardInputBox.setText("");
//	virtualKeyboardInputBox.append(Contents);
    virtualKeyboardPreviousContents = Contents;
    virtualKeyboardInputBox.setText(virtualKeyboardPreviousContents); //以上注释修改
```


###### 2 .Android平台打开输入框没有明确关闭的前提下，按home键进入后台后再打开输入框会再次打开：
解决：虚幻4每次从后台回到前端时候回掉这个函数AndroidThunkJava_ShowHiddenAlertDialog， 如果CurrentDialogType状态没有重置的话，会再次打开输入框，可以在暂停函数的地方修改为：
解决：AndroidThunkJava_ShowVirtualKeyboardInputDialog函数中修改为：
```c++
_activity.runOnUiThread(new Runnable()
{
  public void run()
  {
    switch(CurrentDialogType)
    {
      // this hides the old alert dialog that was used for input
      case Keyboard:
        virtualKeyboardAlert.dismiss();  //修改1
        break;
      case Console:
        consoleAlert.dismiss(); //修改2
        break;
      default:
        Log.debug("ERROR: Unknown EAlertDialogType!");
        break;
    }
  }
});
CurrentDialogType = EAlertDialogType.None; //修改3
```
