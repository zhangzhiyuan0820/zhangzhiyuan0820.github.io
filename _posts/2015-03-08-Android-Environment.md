---
layout: post
title: Android Environment
category: technology
tags:
- 独立博客
- github
- jekyll
keywords: Android
description: 
---

### Environment类



Android应用开发中，常使用Environment类去获取外部存储目录，在访问外部存储之前一定要先判断外部存储是否已经是可使用（已挂载&可使用）状态,

并且需要在AndroidManifest.xml文件中添加外部存储读和写的权限。

<font style="color:red">context.getCacheDir() </font>获取应用程序自己的缓存目录

<font style="color:red">context.getExternalCacheDir()  </font>获取应用程序在外部存储的存储目录

Environment类中提供了几个静态常量用于标识外部存储的状态，这些状态都是String类型

MEDIA_BAD_REMOVAL 在没有挂载前存储媒体已经被移除。

MEDIA_CHECKING 正在检查存储媒体。

MEDIA_MOUNTED 存储媒体已经挂载，并且挂载点可读/写。

MEDIA_MOUNTED_READ_ONLY 存储媒体已经挂载，挂载点只读。

MEDIA_NOFS 存储媒体是空白或是不支持的文件系统。

MEDIA_REMOVED 存储媒体被移除。

MEDIA_SHARED 存储媒体正在通过USB共享。

MEDIA_UNMOUNTABLE 存储媒体无法挂载。

MEDIA_UNMOUNTED 存储媒体没有挂载。

可以通过静态方法getExternalStorageState()来获取外部存储的状态，如果程序需要在外部存储里面读写数据，必须要先判断：

if(Environment.MEDIA_MOUNTED.equals(Environment.getExternalStorageState()) || !Environment.isExternalStorageRemovable())

然后，添加外部存储读和写的权限:

<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE">

<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"></uses-permission></uses-permission>

在Environment中还提供了Android标准目录的路径，以String类型提供。
DIRECTORY_ALARMS 系统提醒铃声存放的标准目录。

DIRECTORY_DCIM 相机拍摄照片和视频的标准目录。

DIRECTORY_DOWNLOADS 下载的标准目录。

DIRECTORY_MOVIES 电影存放的标准目录。

DIRECTORY_MUSIC 音乐存放的标准目录。

DIRECTORY_NOTIFICATIONS 系统通知铃声存放的标准目录。

DIRECTORY_PICTURES 图片存放的标准目录

DIRECTORY_PODCASTS 系统广播存放的标准目录。

DIRECTORY_RINGTONES 系统铃声存放的标准目录。

static File getDataDirectory() 获得data的目录（/data）。
static File getDownloadCacheDirectory() 获得下载缓存目录。（/cache）
static File getExternalStorageDirectory() 获得外部存储媒体目录。（/mnt/sdcard or /storage/sdcard0）
static File getRootDirectory() 获得系统主目录（/system）

除了用Environment获取存储目录之外，还可以通过把路径写死的方式，比如要读取外部存储/mnt/sdcard目录下的文件，可以在程序中直接用全路径，
但是这样做是很不好的，应该Android实在是太开放了，外部存储的目录的什么还是要固件制作商才知道，但是有一点是毋庸置疑的，就是Android框架层里面
已经是指定好了Environment.getDownloadCacheDirectory()的返回路径。所以，尽量用这种方式来获取和存储数据，以免固件厂商不同而造成路径的差异。

Android的实际开发中还用了两个非常重要的缓存目录，一个是应用程序自己的缓存空间，另一个是外部存储为该应该程序提供的缓存空间。有什么差别？

使用过LruCache和DisLruCache的童鞋应该知道。

这两个方法是通过上下文对象Context获取的，只要应用程序被卸载，这两个目录下的文件都要被清空。



















