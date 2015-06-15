---
layout: post
title: 高德地图和百度地图lib冲突解决
category: technology
tags:
keywords: github
description: 
---
分别导入高德地图和百度地图SDK 后报错 :

[2015-06-09 15:06:07 - CMBC_Android_Framework_Test] Error generating final archive: Found duplicate file for APK: assets/lineDashTexture.png
Origin 1: D:\Android\workspace\CMBC_Android_Framework_Test\libs\AMap_3DMap_V2.4.1.jar
Origin 2: D:\Android\workspace\CMBC_Android_Framework_Test\libs\baidumapapi_v3_4_0.jar

该问题是由于高德和百度jar包中 assets /lineDashTexture.png 命名冲突 

解决办法: 

高德地图分为两种 3D 和2D 目前2D不存在冲突问题 更换jar包后完美解决.

如需要使用3D的只能是重新解压开JAR包后删除或重命名该冲突的图片 然后再次压缩成JAR包 这样存在一个问题是 当你的项目中的Map功能使用到当前图片的时候有一定几率导致APP报错  

 