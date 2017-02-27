---
layout: post
title:  "虚幻4中，android第三方类库的接入"
date:   2017-02-27 10:05:00 +0800
categories: "ue4"
tag: ["android", "sdk"]
---
* content
{:toc}

### 虚幻4中安卓sdk的接入, 可以使用 APL (Android Programming Language) 进行第三方库的接入，首先创建一个NAME_APL.XML 文件，该文件包括了库文件的拷贝、权限的写入、activity java文件的修改和添加等一系列android打包和接口修改的内容。不再需要手动去修改代码或者Java层的文件和代码调用接口。
#### 1.创建一个插件并在插件Source目录下创建一个NAME_APL.XML文件，在该插件模块Name.Build.cs文件中调用如下：

```js
public class PluginModuleName : ModuleRules
{
  public PluginModuleNamePluginModuleName(TargetInfo Target)
  {
    // Additional Frameworks and Libraries for Android
    if (Target.Platform == UnrealTargetPlatform.Android)
    {
        PrivateDependencyModuleNames.AddRange(new string[] { "Launch" });
        string PluginPath = Utils.MakePathRelativeTo(ModuleDirectory, BuildConfiguration.RelativeEnginePath);
        AdditionalPropertiesForReceipt.Add(new ReceiptProperty("AndroidPlugin", Path.Combine(PluginPath, "NAME_APL.XML")));
    }
  }
}
```
#### 2.项目中加载该模块
##### 包含头文件目录
```js
PublicIncludePaths.AddRange(
       new string[] {
           "PluginModuleName/Public",
       }
    );

PrivateIncludePaths.AddRange(
        new string[] {
           "PluginModuleName/Private",
        }
    );
```

```js
PublicDependencyModuleNames.AddRange(new string[] { "PluginModuleName" });
PrivateIncludePathModuleNames.AddRange(new string[] { "PluginModuleName" });
```

#### 3.APL_AML中文件结构说明
##### (1). proguardAdditions,代码混淆
```js
<proguardAdditions>
<insert>
  ########################################################################################## sdkname
  -dontwarn com.sdkname.**
  -keep class com.sdkname.** {*;}
</insert>
</proguardAdditions>
```
##### (2). prebuildCopies,目录文件拷贝
```js
<prebuildCopies>
<copyDir src="$S(PluginDir)/../../Libs"
        dst="$S(BuildDir)" />
</prebuildCopies>
```

##### (3). gameActivityImportAdditions,引用插入
```js
<gameActivityImportAdditions>
<insert>
  import com.sdkname.*;
  import com.sdkname.name.*;
</insert>
</gameActivityImportAdditions>
```

##### (4). gameActivityClassAdditions, 成员变量， 函数插入
```js
<gameActivityClassAdditions>
  <insert>
  private AmazonIapManager AmazonManager;
  public void AndroidThunkJava_IapSetupServiceAmazon()
  {
    Log.debug("[JAVA] - AndroidThunkJava_IapSetupServiceAmazon");
    AmazonManager = new AmazonIapManager(this);
    final AmazonPurchasingListener amazonPurchasingListener = new AmazonPurchasingListener(AmazonManager);
    PurchasingService.registerListener(this.getApplicationContext(), amazonPurchasingListener);
    if( AmazonManager == null )
    {
        Log.debug("[JAVA] - Amazon Manager is invalid");
    }
  }
  </insert>
</gameActivityClassAdditions>
```

##### (5). gameActivity[funname]Additions, 在函数中出入内容
```js
<gameActivityOnPauseAdditions>
  <insert>
    if(gvrLayout != null)
    {
      gvrLayout.onPause();
    }
  </insert>
</gameActivityOnPauseAdditions>
```

##### (6). resourceCopies, 文件拷贝，指定源和目标
```js
<resourceCopies>
  <log text="Copying GoogleVR runtime files to staging" />
  <isArch arch="armeabi-v7a">
    <copyFile src="$S(EngineDir)/Source/ThirdParty/GoogleVR/lib/android_arm/libgvr.so"
          dst="$S(BuildDir)/libs/armeabi-v7a/libgvr.so" />
</resourceCopies>
```

##### (7).androidManifestUpdates修改AndroidManifest.xml文件
```js
<androidManifestUpdates>
<!-- Add features -->
<addFeature android:name="android.hardware.sensor.accelerometer" android:required="true" />
<if condition="bSupportDaydream">
  <true>
    <addFeature android:name="android.hardware.vr.high_performance" android:required="true" />
  </true>
  <false>
    <addFeature android:name="android.hardware.vr.high_performance" android:required="false" />
  </false>
</if>

<!-- Add intents -->
<loopElements tag="activity">
  <setStringFromAttribute result="activityName" tag="$" name="android:name" />
  <setBoolIsEqual result="bGameActivity" arg1="$S(activityName)" arg2="com.epicgames.ue4.GameActivity" />
  <if condition="bGameActivity">
    <true>
      <!-- Check for existing intent filter -->
      <setBool result="bHasIntentFilter" value="false" />
      <loopElements tag="intent-filter">
        <setBool result="bHasIntentFilter" value="true" />
      </loopElements>

      <!-- If no intent filter found, add a new one -->
      <if condition="bHasIntentFilter">
        <false>
          <setElement result="newIntentFilter" value="intent-filter" />
          <addElement tag="$" name="newIntentFilter" />
        </false>
      </if>
    </true>
  </if>
</loopElements>
</androidManifestUpdates>
```

##### (7). soLoadLibrary， 在GameActivity.java中指定so库先于libUE4.so加载
```js
<soLoadLibrary>
  <loadLibrary name="gvr" failmsg="GoogleVR library not loaded and required!" />
</soLoadLibrary>
```

#### 4.虚幻4中jni的接口调用
```js

if (JNIEnv* Env = FAndroidApplication::GetJavaEnv())
{
  // Populate some java types with the provided product information
  jobjectArray ProductIDArray = (jobjectArray)Env->NewObjectArray(ProductIds.Num(), FJavaWrapper::JavaStringClass, NULL);
  jbooleanArray ConsumeArray = (jbooleanArray)Env->NewBooleanArray(ProductIds.Num());

  jboolean* ConsumeArrayValues = Env->GetBooleanArrayElements(ConsumeArray, 0);
  for (uint32 Param = 0; Param < ProductIds.Num(); Param++)
  {
    jstring StringValue = Env->NewStringUTF(TCHAR_TO_UTF8(*ProductIds[Param]));
    Env->SetObjectArrayElement(ProductIDArray, Param, StringValue);
    Env->DeleteLocalRef(StringValue);

    ConsumeArrayValues[Param] = IsConsumableFlags[Param];
  }
  Env->ReleaseBooleanArrayElements(ConsumeArray, ConsumeArrayValues, 0);

  static jmethodID Method = FJavaWrapper::FindMethod(Env, FJavaWrapper::GameActivityClassID, "AndroidThunkJava_IapQueryInAppPurchasesAmazon", "([Ljava/lang/String;[Z)Z", false);
  FJavaWrapper::CallBooleanMethod(Env, FJavaWrapper::GameActivityThis, Method, ProductIDArray, ConsumeArray);

  // clean up references
  Env->DeleteLocalRef(ProductIDArray);
  Env->DeleteLocalRef(ConsumeArray);
}
```

### 暂时理解到虚幻4的sdk接入流程和内容基本如上。
