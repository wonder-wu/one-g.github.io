---
layout: post
title: 利用JQuery validate插件实现表单验证
categories: [技术]
tags: [web前端]
date: 2015-12-04 15:32:24.000000000 +09:00
---

####实现效果

![](http://i4.buimg.com/2f2ab00b93baa799.jpg)

####插件介绍
`jQuery Validate` 插件为表单提供了强大的验证功能，让客户端表单验证变得更简单，同时提供了大量的定制选项，满足应用程序各种需求。该插件捆绑了一套有用的验证方法，包括 URL 和电子邮件验证，同时提供了一个用来编写用户自定义方法的 API。

####使用方法
[http://www.runoob.com/jquery/jquery-plugin-validate.html](http://www.runoob.com/jquery/jquery-plugin-validate.html)有详细的使用说明。

###本demo主要代码
demo中主要用正则表达式实现了一个自定义的手机号码验证方式。

```
var mobile = /^(((13[0-9]{1})|(15[0-9]{1})|(18[0-9]{1}))+\d{8})$/
```
可用来验证13、15、18开头的11位中国手机号码。

```
$().ready(function(){
		  $("#testForm").validate({
			    rules: {
			      tel: {
			       required: true,
			       // minlength: 11
			       isMobile:true,
			      },
			     
			      email: {
			        required: true,
			        email: false,
			        isEmail:true,
			      },
			    },
			    messages: {
			      tel: {
			       required: "请输入密码",
			       // minlength: "手机号码长度不能小于11 个数字"
			      },
			      email: {
			      	required:'请输入邮箱',
			      	//email:"请输入一个正确的邮箱",
			      	},
			    }
			});
			
			jQuery.validator.addMethod("isMobile", function(value, element) {       
		     	var length = value.length;   
		 	 	var mobile = /^(((13[0-9]{1})|(15[0-9]{1})|(18[0-9]{1}))+\d{8})$/;   
				 return this.optional(element) || (length == 11 && mobile.test(value));       
			 }, "请正确填写您的手机号码");   
			 jQuery.validator.addMethod("isEmail",function(value,element){
			 	var length = value.length;
			 	var email = /^([a-zA-Z0-9_-])+@([a-zA-Z0-9_-])+((\.[a-zA-Z0-9_-]{2,3}){1,2})$/;
			 	return this.optional(element) ||(length >= 5 && email.test(value));
			 },"请输入正确的邮箱");
		});

```

####演示地址
[http://www.linkwwj.com/demos/form_demo.html](http://www.linkwwj.com/demos/form_demo.html)

