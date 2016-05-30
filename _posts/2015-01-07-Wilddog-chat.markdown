---
layout: post
title: 有趣的Wilddog
categories: [技术]
tags: [web前端]
date: 2016-01-07 15:32:24.000000000 +09:00
---

在学习使用mui + dcloud 5+runtime框架的时，无意中发现了Wilddog这个后端云。

![](http://i2.buimg.com/8a7c8e4c2bb5c5a3.jpg)

现在市面上后端云不少，但是wilddog还是比较特别的，而特别之处就在于其强调的`实时`性，经过测试发现，当数据推到wilddog后，其他监听的客户端几乎能在同时获取到数据变化，分发能力非常强，网络延迟很低。

Wilddog这种实时性，能很好的利用在即时通信甚至简单的实时对战游戏上。


####Wilddog 基本使用方法
#
 1. 在wilddog.com注册
 2. 在个人页面创建一个应用 ![](http://i2.buimg.com/4a3463d3849c7f25.jpg)
 3. 在自己的HTML页面上加入`<script src = "https://cdn.wilddog.com/js/client/current/wilddog.js" ></script>`来获取野狗提供的api
 4. 创建wilddog引用`var ref = new Wilddog("https://<appId>.wilddogio.com/");`
 5. 通过创建的`ref`进行数据操作
	 - `ref.set()`写入与设置json数据
	 - `ref.on()` 监听数据变化

以上便是wilddog的最基本使用方法，当数据在某个客户端`set()`后，便可以通过`on()`监听`change`事件，并向其注册事件处理函数，便可实现简单的多客户端(页面)实时更新数据效果。
 	
####多客户端实时刷新DEMO

#####实现目标

![](http://i2.buimg.com/db15cfe1d0c7673a.jpg)

页面提供发送消息和删除已有消息功能，并且只要是打开了本页面的用户，在发送的和删除的消息时都能即时同步（不用刷新）到其他用户页面上。

####实现思路
数据交给wilddog进行分发与数据同步，前端利用`dom`操作实现后端数据和前端页面显示的同步。

####主要代码


```
	<div class="container" style="text-align: center;">
			<h1>实时刷新demo</h1>
			<div class="col-md-6 col-md-offset-3">
				<ul class="list-group">
				  <li class="list-group-item">基石</li>
				</ul>
				<div class="form-inline form-group">
					<input type="text" class="form-inline form-control" name="content" id="content" placeholder="发送内容">
				<button type="none" class="btn btn-default" id="send">发送</button>
  				</div>
			</div>
		</div>
```


```

		$().ready(function(){
			//引入wilddog
			var data = new Wilddog('https://liverefresh.wilddogio.com/test/');
			//发送
			var send = $('#send');
			send.click(function(){
				var nowTime = new Date();
				var timeStr = nowTime.Format("yy-MM-dd hh:mm:ss");
				var sendText = $('#content').val();
				if (sendText) {
					//传入后端
					data.child(sendText).set(
						{
							'content':sendText,
							'time':timeStr,
						}
					);
				}else{
					alert('请输入内容');
				}
			});
			//删除
			$(document).on('click','.remove',function(){
				var removeContent = $(this).siblings('span').text();
				data.child(removeContent).set(null);
			});
			
			//处理后端数据变化
				data.on("child_added",function(snapshot){
					var newData = snapshot.val();
					//将数据更新到页面上
					//alert(newData.content);
					var toAdd = "<li class='list-group-item'><span>"+newData.content+"</span><label class='remove' style='display:inline-block ;float:right;color:#C9302C;cursor:pointer'>删除</label></li>";
					$('.list-group-item').first().before(toAdd);
				});
				data.on("child_removed",function(snapshot){
					var removeData = snapshot.val();
					//将数据更新到页面上
					var toRemove = removeData.content;
					//alert(toRemove);
					$('.list-group-item').each(function(){
						if (($(this).children('span').text())==toRemove) {
							$(this).remove();
						}
					});
				});
		});

```

####总结
利用baas服务很大程度上减少后端编程的工作量。如果用传统的mysql+php写这个demo，我需要部署后端服务器，用php连接前端提交的表单和后端的数据库，当涉及到实时内容显示时，需要用到ajax向后端轮询是否有数据改变。而通过baas服务，简单的通过js就完成了这个繁杂的过程。


####演示地址
[http://www.linkwwj.com/demos/liveRefresh.html](http://www.linkwwj.com/demos/liveRefresh.html)


