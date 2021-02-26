---
title: UnityShader学习笔记
top: false
cover: false
toc: true
mathjax: true
date: 2021-02-26 17:34:28
password:
summary:
tags: Unity
categories:
---

## saturate
返回范围[0,1]的值，如下所示：
- 如果x小于0，则返回0
- 如果x大于1，则返回1
- 否则返回x

函数原型：
```cpp
float saturate（float x）
{ 
  return max（0，min（1，x））; 
}
```
https://developer.download.nvidia.com/cg/saturate.html