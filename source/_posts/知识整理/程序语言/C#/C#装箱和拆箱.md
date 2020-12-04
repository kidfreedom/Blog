---
title: C#装箱和拆箱
top: false
cover: false
toc: true
mathjax: true
date: 2020-06-26 13:57:49
password:
summary:
tags:
- 面试
categories:
- 程序语言
- C#
---

## 定义
装箱是将`值类型`转换为`引用类型`。
拆箱是将`引用类型`转换为`值类型`。

## 装箱的内存操作
1. 在托管堆上分配一段内存(大小为值类型实例大小加上一个方法表指针和一个SyncBlockIndex)。 
2. 将值类型的数据拷贝到刚刚分配的内存中。
3. 返回托管堆中新分配对象的地址。这个地址就是一个指向对象的引用了。

> 进行一次装箱要进行`分配内存`和`拷贝数据`。

## 拆箱的内存操作
1. 首先获取托管堆中属于值类型那部分字段的地址，这一步是严格意义上的拆箱。
2. 将引用对象中的值拷贝到位于线程堆栈上的值类型实例中。

> 进行一次拆箱要进行`拷贝数据`。

## 参考
https://www.bilibili.com/video/BV1eW411v76V?from=search&seid=11233487436428803311
https://blog.csdn.net/qiaoquan3/article/details/51439726