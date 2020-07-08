---
title: Unity Ecs 使用问题
top: false
cover: false
toc: true
mathjax: true
date: 2020-07-08 22:23:57
password:
summary:
tags: 
- Ecs
- Unity
categories:
---

## JobSystem使用问题
1. 函数体内断点不能命中
2. 函数体内不能使用C#的容器只能使用Unity的NativeContainer容器，而NativeHashMap不能是使用`集合初始化器`，如果涉及到读取静态数据的时候将会很麻烦。



