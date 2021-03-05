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


## Cg/HLSL函数

### Color

`float4 name ：COLOR` ：通过使用这种语法，变量名将`name`包含顶点的颜色

### saturate
https://developer.download.nvidia.com/cg/saturate.html

函数原型：
```cpp
float saturate（float x）
{ 
    return max（0，min（1，x））; 
}
```
返回范围[0,1]的值，如下所示：
- 如果x小于0，则返回0
- 如果x大于1，则返回1
- 否则返回x

### step
https://developer.download.nvidia.com/cg/step.html

函数原型：
```cpp
float3 step(float3 a, float3 x)
{
    return x >= a;
}
```
- 如果x大于等于a，则返回1
- 否则返回0
