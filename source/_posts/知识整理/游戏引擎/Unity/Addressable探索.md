---
title: Addressable探索
top: false
cover: false
toc: true
mathjax: true
date: 2020-12-08 16:30:59
password:
summary:
tags: 
- Unity
- Addressable
categories: 游戏引擎
---

> 测试环境：
> Unity2019.4.15f1c1
> Addressable1.16.13
> Apache2.4.46(win64)

## 安装Addressable
1. 打开菜单Widnow-->Package Manager
![](/images/Addressable/a1.png)
2. 找到Addressable下载1.16.13版本

## 安装Apache
1. 下载Apache，地址：http://httpd.apache.org/download.cgi
2. 解压缩到`D盘`
3. 打开`D:\Apache24\conf\httpd.conf`文件
4. 找到`Define SRVROOT`后面是Apache24的解压目录，这里是`Define SRVROOT "D:\Apache24"`
5. 找到`ServerName`后面是服务器的url，这里是`ServerName http://localhost:8080/`
   
   