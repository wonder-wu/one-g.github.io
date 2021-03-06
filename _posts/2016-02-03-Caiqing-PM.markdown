---
layout: post
title: 招聘平台才情数据分析系统的设计简单记录
categories: [产品]
tags: [产品]
date: 2016-02-03 15:32:24.000000000 +09:00
---
### 前言
近期参与设计了一个招聘平台才情数据分析系统的产品原型，从需求分析，功能确定，到生成PRD和制作高保真原型，整个设计和思考的过程学到了很多东西，受益良多。遂把思考和实践的过程记录下来，以便以后查阅。

###开始的困惑
此系统的设计目的是将招聘网站上沉淀下来的大量数据整理出来，形成一个可以帮助求职者，用人单位与政府人力资源机构进行辅助分析与决策的系统。

当突然接到这个需求时我是很困惑的，主要困惑如为：
>我对招聘领域完全不了解，我该如何快速进入状态着手设计？

针对这个问题，我是这么解决的

#
 - 研究该招聘网站，整体把握用户求职和企业招聘行为图，记录下作为普通用户与用人单位使用该系统时，会用到的数据字段（如求职者注册时会用到`学历`、`工作经验`等，用人单位会用到`所属行业`，`参考月薪`等字段）
 - 寻找相似的系统，寻找设计参考
 - 阅读中国人力资源网与其他专业机构的人力资源分析研报，了解人力资源的分析方法

通过上述摸索后，对该领域有了一定的了解，结合给定的需求，我开始构思第一版的设计文档。

###需求确定
一开始给定的需求是方向性的概括需求，为了能更好的完成设计，需要把需求更加细化具体化，我先设定了各类用户的使用情景，并自己给出了具体需求。然后再去接触用户，通过用户的反馈来进行修正。

![](http://i4.buimg.com/7990ef1a220358dc.jpg)

###搭建基本框架
搭建框架时依然是站在用户角度使用的角度进行构思，普通用户和用人单位需要的是直观的数据分析，人力资源机构需要的是大而全深度数据处理。而这些数据的组织和分析在这三种用户角度似乎又有所重复，如何能做到数据展示得恰到好处而又不繁琐呢？所以，信息如何展示与组织成为这个阶段的主要内容。

最后我通过划分信息展示的的基本单位解决了这个问题。

#
 - 针对普通用户，以`职位`为基本单位来组织数据。
 - 针对普通以上的用户，以`基本数据`为基本单位来组织数据。



###高保真原型
一份详细的功能说明+高保真原型对开发速率的提升是非常大的，主要的决策在高保真原型完成时就已经定好，大大减少了在开发过程中的反复修改和沟通不清晰的问题。

###系统展示

![](http://i2.buimg.com/3b7f870b560e0fde.jpg)

![](http://i2.buimg.com/fae01a26b320710b.jpg)

![](http://i2.buimg.com/1c58ea7cadd6d3a6.jpg)

![](http://i2.buimg.com/544086e8bec5aeb3.jpg)






