---
title: unity与android之间的相互调用
date: 2017-06-06
lastmod: 2017-07-04
tags:
- unity3d
categories:
- Mobile
---

# unity调用android接口的方法
代码示例：

```csharp
AndroidJavaClass jc = new AndroidJavaClass("com.unity3d.player.UnityPlayer");
AndroidJavaObject jo = jc.GetStatic<AndroidJavaObject>("currentActivity");
jo.CallStatic("static_foo");
jo.Call("foo");
```

* AndroidJavaClass是Java类的指针，如果调用该类的静态方法可以直接调用CallStatic方法；
* AndroidJavaObject是Java的对象，通过获取的对象才能用Call方法调用其成员方法。

# android调用unity接口
代码示例：

```java
public static void Login()
{
	UnityPlayer.UnitySendMessage("GameObject", "Foo", params);
}
```

* UnitySendMessage中的第一个参数是unity里的gameobject，第二个是挂在这个go上的脚本的接口名，因此原则上需要被调用的go只挂一个script比较好。