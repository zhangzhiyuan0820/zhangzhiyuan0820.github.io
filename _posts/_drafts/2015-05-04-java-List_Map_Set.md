---
layout: post
title: List Map Set 之间的关系和区别
category: technology
tags:
keywords: github
description: 
---

List : 元素是有顺序的放入,且 元素可以重复
Map : 元素按照键值对存放,元素存放没有顺序 
Set : 元素无放入顺序, 且元素不可重复, 元素在Set中的位置是固定的 由HashCode决定.

List 接口中有三个实现类 LinkList ArrayList Vecter 
LinkList 底层是基于链表实现, 链表内存存放无序, 每个元素地址不但存放数据本身还存储着下一个元素的地址,链表增删块但查询慢

ArrayList 和 Vecter 的区别是 在于 ArrayList 是非线程安全 效率较高, Vecter 是线程安全的 效率较低

Set接口有两个实现类 HashSet   LinkedHashSet 
SortedSet 接口有一个实现类 底层由 平衡二叉树实现
Query 接口有一个实现类 LinkList 

Map 接口有三个实现类 HashMap HashTable LinkHashMap
HashMap 非线程安全 高效 支持null 
HashTable 线程安全 低效 支持null
LinkHashMap 链表结构 增删快 查询慢


























