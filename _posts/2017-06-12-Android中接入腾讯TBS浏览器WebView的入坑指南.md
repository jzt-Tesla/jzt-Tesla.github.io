---
layout: post
title: Android中接入腾讯TBS浏览器WebView的入坑指南
subtitle: 腾讯TBS
date: 2017-06-12
author: 霜刃西瓜
header-img: img/post_bg_Android.jpg
catalog: ture
tags:
  - Android
---

###  为什么不使用原生的webview？
最近公司的项目接入了webview，但是坑巨多无比，尤其是其内存泄露。所以我在想是否可以有第三方封装了webview。
###  比较Crosswalk与TBS服务
1.Crosswalk这玩意儿我没用过，据说是很流畅和强大，但是有一点是我暂时无法接受的，接入Crosswalk的话会导致APP的体积增大20M左右 ~ 所以我就放弃了，不过大家想研究的话那就自己去百度吧 ！（嘿嘿，微笑脸） 

2. TBS，腾讯出品，其实有点坑，本来也准备接入一下支付宝和淘宝都用的UC的内核的Webview的，但是我看了一下，需要审核，有点麻烦 ~

### 开始配置
1. 首先需要去网站[腾讯TBS浏览服务](https://x5.tencent.com/tbs/technical.html#/) 进行注册，吐槽一点，需要验证身份证信息，坑爹的一比 
2. 参考一下[TBS接入文档](https://x5.tencent.com/tbs/guide/sdkInit.html) 不过里面讲解的比较啰嗦，不太清楚。
    a. 简单来讲的话就是 首先下载SDK和官方DEMO：[完整版SDK和官方DEMO](https://x5.tencent.com/tbs/sdk.html) ，然后进行导入jar包和so文件 。jar包直接复制官方DEMO里面的，然后导入library。so文件的话直接复制Demo里面的文件夹**jniLibs**，到**src/main/jniLibs**。

  ![jniLibsの位置](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcwNjEzMDk1MTIxMzQz?x-oss-process=image/format,png)

 b. 不过在这里<font color="#BE1A21">有个很坑的点</font>是需要对so文件进行配置的，由于X5暂时不提供64位so文件，但是现在<font color="#F15A24">绝大部分手机都是64位的</font>，所以为了保证64位手机能正常加载x5内核，需要进行配置。配置方法参考：[64位手机无法加载x5(libmttwebview.so is 32-bit instead of 64-bit)](https://x5.tencent.com/tbs/technical.html#/detail/sdk/1/34cf1488-7dc2-41ca-a77f-0014112bcab7)。

```
  defaultConfig {
        applicationId "com.jzt.mytbsdemo"
        minSdkVersion 15
        targetSdkVersion 25
        versionCode 1
        versionName "1.0"
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
        //配置so文件
        ndk {
            abiFilters "armeabi", "armeabi-v7a", "x86", "mips"
        }

//注意：这里之所以将这以下的代码注释掉，是因为我们已经到src/main/jniLibs里面导入了so文件，如果是在libs里面导入的so文件的话，则用以下代码 ！

    //android studio默认so文件加载目录为:src/main/jniLibs
    //如在module的build.gradle按照如下方式,自定义了so文件加载目录请确保对应目录下只有armeabi目录
    //    sourceSets {
    //        main{
    //            jniLibs.srcDirs = ['libs']
    //        }
    //    }
```

c. **OVER**了，于是的话就这样将SDK配置完成了。

3.添加权限：

```


<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />

<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />

<uses-permission android:name="android.permission.INTERNET" />

<uses-permission android:name="android.permission.READ_PHONE_STATE" />

```

4 . 配置Application

```
public class BaseApplicatiom extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        //初始化X5内核
        QbSdk.initX5Environment(this, new QbSdk.PreInitCallback() {
            @Override
            public void onCoreInitFinished() {
                //x5内核初始化完成回调接口，此接口回调并表示已经加载起来了x5，有可能特殊情况下x5内核加载失败，切换到系统内核。

            }

            @Override
            public void onViewInitFinished(boolean b) {
                //x5內核初始化完成的回调，为true表示x5内核加载成功，否则表示x5内核加载失败，会自动切换到系统内核。
                Log.e("@@","加载内核是否成功:"+b);
            }
        });
    }
}
```

```![这里写图片描述](http://img.blog.csdn.net/20170613095044780?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMDMxMjk0OQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
//别忘了在Manifest里面配置Appliaction的名字 ！
 <application
        android:name=".BaseApplicatiom"
```
5 .  Application里面配置key ，name一定得设置为： QBSDKAppKey

```
<meta-data  
android:name="QBSDKAppKey"  
android:value="PpeRpHjyzL5vTlc9LuNrRmHM" />
    </application>  
```

6. 其它的都和Webview一样了，不过我看官方文档说的是只要出现<font color="#F15A24">水滴就是成功了，其实是错误的 ! ! ! </font>我的好几个APP都没有用TBS，但是**也是水滴的**。于是我找了好久，终于找到了这个测试的方法，绝对是niubility的~  [X5内核加载问题自动检测工具发布啦](http://bbs.mb.qq.com/thread-1944983-1-1.html)

啦啦啦啦。。。下班了，明天再补充吧！！！

咳咳，回来了，继续啦啦啦啦 ~ 

7 . 经过测试以后得到的是这个，证明已经接入成功了！：

![最后结果是](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcwNjEzMTAwMjE5NDcz?x-oss-process=image/format,png)


**----------------------------------------------------------------------------------------------------**

### 是时候放出源代码了！------> [TBS不官方不坑人の源代码](https://github.com/jzt-Tesla/MyTencentTBSDemo)

**----------------------------------------------------------------------------------------------------**

### About QA：

[无法接入成功](接入tbs) 
[SDK接入问题](https://x5.tencent.com/tbs/technical.html#/sdk) 
[判断接入问题](http://bbs.mb.qq.com/thread-1444656-1-1.html)
[腾讯X5内核的集成和使用](http://blog.csdn.net/langxingtianxi/article/details/51774347)
[Android WebView使用总结](https://zhuanlan.zhihu.com/p/23388355?refer=siyehua)

**----------------------------------------------------------------------------------------------------**
### Expand QA：
[AgentWeb](https://github.com/jzt-Tesla/AgentWeb)
[Android WebView使用总结](https://zhuanlan.zhihu.com/p/23388355?refer=siyehua)

**----------------------------------------------------------------------------------------------------**

