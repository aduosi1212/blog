---
layout: post
title:  "虚幻4中，android第三方类库的接入"
date:   2017-08-02 14:05:00 +0800
categories: "ue4"
tag: ["微信", "sdk"]
---
* content
{:toc}

虚幻4接微信sdk，官网地址是：https://open.weixin.qq.com/

#### 1.拷贝库文件：
微信的sdk资源库文件只有一个wechat-sdk-android-with-mta-1.3.4.jar，现在是这个名字，以后不知道，从官网上可以下到，把这个文件拷贝到android的libs文件夹里。

#### 2.创建回调接口：
在应用包名文件夹内创建一个新的文件夹wxapi，并在里面生成一个名称为WXEntryActivity的Activity，并实现接口IWXAPIEventHandler，这个可以用它官网的修改，在onCreate函数中实现以下功能, 用来判断失败的话就关掉Activity
```c++
api = WXAPIFactory.createWXAPI(this, APP_ID, false);
try {
        boolean ret = api.handleIntent(getIntent(), this);
        if (!ret)
        {
            finish();
        }
    } catch (Exception e) {
        e.printStackTrace();
    }
```
重写以下函数，否则回调不到
```c++
@Override
protected void onNewIntent(Intent intent) {
    super.onNewIntent(intent);
    setIntent(intent);
    api.handleIntent(intent, this);
}
```
重写以下接口，微信调用第三方应用的接口：
```c++
@Override
public void onReq(BaseReq req) {
      WXMediaMessage wxMsg = ((ShowMessageFromWX.Req)req).message;		
      WXAppExtendObject obj = (WXAppExtendObject) wxMsg.mediaObject;
      StringBuffer msg = new StringBuffer();
      msg.append("description: ");
      msg.append(wxMsg.description);
      msg.append("\n");
      msg.append("extInfo: ");
      msg.append(obj.extInfo);
      msg.append("\n");
      msg.append("filePath: ");
      msg.append(obj.filePath);
      Log.d("UE4", "[WeChat]" + "onReq = " + msg.toString());
//    Log.debug("[WeChat]" + "onReq = " + msg.toString());
}
```

重写以下接口，第三方应用调用微信后的响应接口：
```c++
@Override
public void onResp(BaseResp resp) {
    String result = "";
    switch (resp.errCode) {
        case BaseResp.ErrCode.ERR_OK:
            result = "errcode_success";
            break;
        case BaseResp.ErrCode.ERR_USER_CANCEL:
            result = "errcode_cancel";
            break;
        case BaseResp.ErrCode.ERR_AUTH_DENIED:
            result = "errcode_deny";
            break;
        case BaseResp.ErrCode.ERR_UNSUPPORT:
            result = "errcode_unsupported";
            break;
        default:
            result = "errcode_unknown";
            break;
    }
    Log.d("UE4", "[WeChat] onResp: " + result + "---resp.errCode:"+ resp.errCode + "---resp.errStr = " + resp.errStr);
    finish();
}
```
这样处理就可以实现微信的调用和回调
现在测试了下回调的返回结果是-6，官网没找到这个错误码，网上说是因为签名问题，等注册应用后再测试下。
