---
layout: post like 
title: 关注力
category: technology
tags:
keywords: github
description: 
---
每个Android应用程序都运行在一个dalvik虚拟机进程中，进程开始的时候会启动一个主线程( Main Thread )

主线程负责处理和ui相关的事件，因此主线程通常又叫UI线程。

而由于Android采用UI单线程模型，所以只能在主线程中对UI元素进行操作。

handler : 消息循环从消息队列取出消息后对消息进行处理 

message: 用于消息的表示 

messageQueue : 消息队列 用于存放Handler发送过来的消息 并非实际存放在某个地方 而是将Message以链表的形式串联起来,等待Looper的抽取

looper : 用于循环取出消息进行处理

整个流程步骤 是 

一个Message 经由Handle 的发送 

MessageQueue 以链表的形式 串联起来

looper 的抽取 

发送到handler 


























