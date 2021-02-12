---
title: 外部链接打开安卓应用的方法
date: 2017-06-15
lastmod: 2017-07-04
tags:
- android
- unity3d
- scheme
categories:
- Mobile
---

首次正式接触安卓工程，正好要做个点击外部链接（分享链接）打开游戏的功能碰到了点问题总结一下。

# unity导出android工程
网上搜索到的方法都是需要更改android工程的manifest文件，因此首先需要的是将unity工程导出android工程。我使用的是新的gradle方法，因此需要安装[Android Studio](https://developer.android.com/studio/index.html)
1. 打开unity的**Build Settings**，如图：![asset_img](switch-platform.jpg)
2. 选择**Android**平台并点击**Switch Platform**；
3. 点击**Player Settings**；
检查是否设置了**Other Settings**的**Package Name**，gradle的导出方式必须填写这项；
在**Publishing Settings**中可以先加上签名信息（gradle方式发布release包必须有签名）。
4. 回到**Build Settings**将**Build System**改为**Gradle (New)**并勾选上**Export Project**后点击**Export**，选择想要导出的文件夹点击**Select Folder**后工程就会被导出。

<!-- more -->

# 正常编译android工程需要修改的内容
unity导出时可能一些信息已经过时需要自己更新。
1. **build.gradle**是gradle自动化编译过程的配置文件，其中可以找到类似于

```gradle
buildscript {
	...
	dependencies {
		classpath 'com.android.tools.build:gradle:2.3.2'
	}
}
```

的代码，在[Android Plugin for Gradle](https://developer.android.com/studio/releases/gradle-plugin.html#revisions)中确认最新的工具版本号将**2.3.2**替换为最新的；
2. 打开Android Studio选择**Import project (Eclipse ADT, Gradle, etc.)**选择导出工程的build.gradle文件后点击**OK**，等待gradle执行完成；
3. 这步是可选操作，构建完成后会在工程目录下生成gradle/wrapper的目录结构，其中存放的是gradle工具及配置信息。如果想要将gradle更新到最新版本，可以用文本工具打开**gradle-wrapper.properties**文件，将**distributionUrl**项的值改为[最新包的地址](https://services.gradle.org/distributions/)的完整包地址，如**https\://services.gradle.org/distributions/gradle-4.0-all.zip**，其中冒号前需要加转义符。

# 如何让外部链接点击后启动游戏
需要在你的启动activity中加入**intent-filter**字段，首先这个字段是可以重复的，一开始写在一起于是怎么打包都无法打开游戏……unity导出的工程只有一个activity——**UnityPlayerActivity**，然后列出几个**intent-filter**字段下的关键属性：
* 添加**action**属性设置值为"android.intent.action.VIEW"；
* 添加**category**属性设置值为"android.intent.category.DEFAULT"，这样可以接受外部任意输入；
* 添加**category**属性设置值为"android.intent.category.BROWSABLE"，这样可以通过浏览器启动；
* 添加**data**属性，我只用到了**android:scheme**和**android:host**两个，其实**android:scheme**就够了，具体这些参数什么含义以及填写规则可以查看[Android 使用Scheme实现从网页启动APP](http://www.jianshu.com/p/1cd02fe1810f)
***

完成后的代码大致如下：

```xml
<activity>
	<intent-filter>
		<!-- 原有的内容 -->
	</intent-filter>
	<intent-filter>
		<action android:name="android.intent.action.VIEW" />
		<category android:name="android.intent.category.DEFAULT" />
		<category android:name="android.intent.category.BROWSABLE" />
		<data android:scheme="myscheme" android:host="myhost" />
	</intent-filter>
</activity>
```

于是再编写个简单html文件测试功能：

```html
<html>
<a href="myscheme://myhost/?param=value">start my app!</a>
</html>
```

成功~

# 总结
其实还是没做过android项目对AndroidManifest.xml的配置方法不熟，走了很多弯路，安装完后无法打开app、启动后闪退等……

# 说明
微信的WebView貌似受到腾讯的限制scheme的方法不起效，还需尝试通过外部网页调用的方法。
另外，iOS的方法类似，只需在info.plist中配置**URL Identifier**(类似Android的host)和**URL Scheme**即可。