---
layout: post
title: Unity学习之模仿tilt to live小记
categories: [技术]
tags: [unity]
date: 2016-04-15 15:32:24.000000000 +09:00
---
### 前言
一直以来对游戏开发行业都十分向往，这次借离职的机会，终于静下心来学习了一下如何用unity开发游戏。

在了解了unity的基本特性后，我（部分）仿制了一个我非常喜欢的游戏，tilt to live（重力存亡）。这篇博文主要记录一下我在开发过程中遇到的比较有意思的问题。

[![show.gif](http://imgchr.com/images/show.gif)](http://imgchr.com/image/P39)

apk下载地址：[http://www.linkwwj.com/copy-tilt-to-live.apk](http://www.linkwwj.com/copy-tilt-to-live.apk)

源码见我的github: [github.com/wonder-wu](github.com/wonder-wu)



###重力感应控制移动

游戏的输入方式为重力感应，通过unity提供的接口`Input.acceleration`可以很方便的获取手机的重力感应偏移数据，`acceleration`提供了`xyz`三个方向的速度，根据手机倾斜程度的不同，会返回相应方向下的值，值的范围在`[-1,1]`之间。作为平面移动的游戏，`xy`便足够了。只要在`update`中获取相应的值，再处理到位移上，同时将玩家控制的物体方向旋转到此方向就行，很简单。


```

	void Update()
	{
		//移动
		float m_accelX = Input.acceleration.x;
		float m_accelY = Input.acceleration.y;
		Vector3 _moveMent = new Vector3 (m_accelX, m_accelY, 0f);
		Vector3 _moveTo = transform.position + _moveMent * SPEED;
		transform.position = _moveTo;
		//旋转
		float desiredRotation = Mathf.Atan2 (targetHeading.y, targetHeading.x * Mathf.Rad2Deg;
		Quaternion qd = Quaternion.Euler (0f, 0f, desiredRotation);
		transform.rotation = Quaternion.Slerp (transform.rotation, qd, Time.deltaTime * 5f);
	}

```


>到这里一切都很美好，但是！接下来坑便出现了。

这个坑就是重力感应数据的`敏感度`问题或者叫`噪声`问题。 当大幅度倾斜手机，让物体高速移动时，转向和位移都表现的相当完美，而当倾斜幅度趋缓时，会发现，基于重力感应数据驱动的物体发生了无法控制的转向抖动。
这让游戏的基本玩法之一：接到道具，瞄准，射击 完全无法进行。

而出现这个问题的根本原因有一下几点：

#
 - 手机重力感应精度不佳
 - 人的手是会抖动的
 - 在低倾斜的状态下，由于 `xy`基数很小，轻微的抖动都会造成其在某一方向上的数值的相对增长变大，造成转向方向突变，且在近水平状态下时，轻微的抖动会造成`xy`的数值在+与-之间摇摆，导致转向方向突变。

要解决这个问题，主要通过以下方式：

#
 - 对原始重力感应数据加入`Low-pass Filter低通滤波` , 过滤突变的噪音数据
 - 设定一个`速度死区`，让物体在这个速度之下时，不再旋转
 

```

	//低通滤波
	//m_FilterFactor为过滤因子，越小过滤效果越好
	m_accelX = (Input.acceleration.x * m_FilterFactor) + (m_accelX * (1f -m_FilterFactor));
	m_accelY = (Input.acceleration.y * m_FilterFactor) + (m_accelY * (1f -m_FilterFactor));
	MoveSelf();
	if(m_velocity > DEADZONE)
	{
		RotateSelf();
	}

```

这样重力感应问题便几乎完美解决了。

###手机上Animator的性能问题

在小红点敌人生成的时候，我做了一个闪烁的动画效果，`Animator`用来控制这个效果的播放与停止。当我把游戏安装到安卓上，并打开`profiler`观察性能时发现，由于游戏中有大量的小红点敌人， `Animator`占用了大量的处理时间。我的解决方式是在播放完动画后，不需要再用到`Animator`后，手动将其`enable = false`掉。

经过测试，性能确实有提升。

###slerp插值函数的使用

`slerp`球面线性插值，使用方法为`Quaternion.Slerp(Quaternion origin,Quaternion to,float t)`。值得一说的是此函数在插值的时候，可以找到最小旋转方向，十分方便。比如80°转到20°它会自动帮你顺时针转过去，而不会选择逆时针这个大转向。


###Coroutine的使用

不得不说，unity的coroutine实在太好用了，避免了在update中堆入大量的状态控制代码。

使用方法为：

```

	//定义一个处理coroutine的函数
	IEnumerator MyCoroutine()
	{
		//do balabala
		...
	 	//等待1s
		yield return new WaitForSeconds(1f);
		//继续 do balabal 
		...
		//下一帧继续执行
		yield return null;
		//do balala
		...
		
	}

	//在某个函数中调用即可
	StartCoroutine(MyCoroutine());

```











