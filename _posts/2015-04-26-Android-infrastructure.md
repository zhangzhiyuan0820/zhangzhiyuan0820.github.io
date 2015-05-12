---
layout: post
title: android 程序初期项目设计
category: technology
tags:
- android
- 程序初期
- 项目设计
keywords: Android
description: 
---
### android 程序初期项目设计


1 框架说明

	定义基于http协议通讯文档
	
	建立json数据传输解析格式
	
	以及字符编码(UTF-8) 
	

APP主要模块设计

	framework

	view试图

	业务逻辑模块

	本地数据模块

	工具类

	Model

	Server

	Service

	Adapter

	Activity

	Sharedpreferences

	Utils

	
以及在项目开发过程中共同特性设计

	编码一直

	整体APP风格 例如:UI,色调,ListView

	项目结构统一调用

	以及各种开发文档

	
Widget 

	TextView

	EditText

	Button

	Title Bar

	自定义view

	尽量做到UI统一可复用性强
	
	公共组建的复用<!-- 公共组建的复用 公共组建的复用 重要的事情说一百遍都不够啊-->
	
	尤其是一些共同的组件 只要在项目中出现次数超过3次的都采用include 引入 或直接做成view 根据项目情况看

	
Adapter Items

	项目中所要显示的item
	
	以及初期一些优化

	
BaseActivity

	需设计一个BaseActivity 之后所有的Activity 均继承于此

	onCreate 方法里增加 以及它们的抽象类 如果需要可以封装点击事件	findViews();

	intViews();

	setlisteners();

	
设计application类

	存储全局数据;

	
设计异常类

	设计自义定异常

	以及系统抛出的异常类

	标注的使用
	
	在项目过程中如有不同的类 不要删除 标注为@Depercated 即可

	
项目需使用的开源框架和开放接口
	
	Android-Universal-Image-Loader
	
	xutils
	
	BaiduLBS_Android








