---
title: 创建单件的方法
date: 2017-06-07
lastmod: 2017-06-12
tags:
- singleton
categories:
- C++
---

通常创建单件的代码都是长这样的：

```c++
class USingleton
{
public:
	static USingleton* Instance()
	{
		if (Instance == null)
		{
			Instance = new USingleton();
		}
		return Instance;
	}

private:
	USingleton() {}

	USingleton* Instance;
}
```

<!-- more -->

最近看了《游戏编程模式》中对于单件的描述发现了个更安全的写法：

```c++
class USingleton
{
public:
	static USingleton& Instance()
	{
		if (Instance == null)
		{
			Instance = new USingleton();
		}
		return *Instance;
	}

private:
	USingleton() {}

	USingleton* Instance;
}
```

这样返回的是单件类实例的引用而不是指针。
以下是个更现代的版本：

```c++
class USingleton
{
public:
	static USingleton& Instance()
	{
		static USingleton Instance;
		return Instance;
	}

private:
	USingleton() {}
}
```

>C++11保证一个局部静态变量的初始化只进行一次，哪怕是在多线程的情况下也是如此。所以，如果你有一个现代C++编译器的话，这份代码是线程安全的。