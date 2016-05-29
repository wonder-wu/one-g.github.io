---
layout: post
title: jQuery实现Slider左右图片滑动
date: 2016-12-02 15:32:24.000000000 +09:00
---

####效果
![](http://i2.buimg.com/23954bdfec5bcba3.jpg)

####思路原理
用特定大小的父级`div`标签包裹多个横向排列的`li`图片，同时为父级标签设置`overfolw:hidden`属性，使用户只能看到暴露出来的特定`li`图片，在点击左右按钮时，改变`li`的`left`属性，使其相对于原先位置左右移动，并利用jQuery提供的`animate`函数生成连续的动画效果。

值得注意的是，为了实现“无限滑动”效果，即滑动到最后一张图片时，再往下变会滑动到第一张的效果，需要在每次滑动时，根据条件改变各个`li`的`left`相对位置。

```
	$().ready(function(){
		//轮播函数
			(function(){
				$('.one ul li').not(':first').css({left:400});
				var i=0;
				var n=3;
				var li = $('.one ul li');
				//注册左按钮
				$('#right-bar1').click(function(){
					$('.one ul li').not(':eq('+(i)+')').css({left:400});
					if (!li.is(':animated')) {
						i++;
						if (i<n) {
							li.eq(i-1).animate({left:-400},'slow',function(){
								li.eq(i-1).css({left:400})
							});
							li.eq(i).animate({left:0},'slow');
						}
						else{
							li.eq(i-1).animate({left:-400},'slow',function(){
								li.eq(i-1).css({left:400})
							});
							li.eq(0).animate({left:0},'slow');
							i=0;
						}
					}		
				})
				//注册右按钮
				$('#left-bar1').click(function(){
					
					$('.one ul li').not(':eq('+(i)+')').css({left:-400});
					if (!li.is(':animated')) {
						i--;
						if (i>-1) {
							li.eq(i+1).animate({left:400},'slow',function(){
								li.eq(i+1).css({left:-400})
							});
							li.eq(i).animate({left:0},'slow');
						}
						else{
							li.eq(i+1).animate({left:400},'slow',function(){
								li.eq(i-1).css({left:-400})
							});
							li.eq(n-1).animate({left:0},'slow');
							i=n-1;
						}
					}				
				})
			})('demo1');
		});
```

####演示地址
[http://www.linkwwj.com/demos/slider_demo.html](http://www.linkwwj.com/demos/slider_demo.html "图片左右滑动轮播演示")
