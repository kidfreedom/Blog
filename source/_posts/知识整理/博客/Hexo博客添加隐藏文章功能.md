---
title: Hexo博客添加隐藏文章功能
top: false
cover: false
toc: true
mathjax: true
date: 2020-12-04 12:29:05
password:
summary:
tags:
categories: 博客
---

为 Hexo 博客添加隐藏文章功能
https://printempw.github.io/hexo-plugin-to-make-posts-sage-unlisted/

于是我写了一个 Hexo 插件 hexo-hide-posts 来实现这个需求（网上也有一些关于 Hexo 隐藏文章的教程，不过一般都要求修改主题文件，还是我这样写个插件更通用一些）。它的功能如下：
在博客的所有文章列表中隐藏指定的文章（包括首页、存档页、分类标签、Feed 等）；
被隐藏的文章依然可以通过文章链接直接访问（比如 https://hexo.example/{slug}/）；
除非知道链接，任何人都无法找到这些被隐藏的文章。

插件下载地址：
https://github.com/printempw/hexo-hide-posts