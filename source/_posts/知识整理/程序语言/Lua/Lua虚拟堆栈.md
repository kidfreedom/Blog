---
title: Lua虚拟堆栈(virtual stack)
top: false
cover: false
toc: true
mathjax: true
date: 2020-12-05 16:36:24
password:
summary:
tags: 
- Lua C API 
categories: 程序语言
---

## Lua虚拟堆栈(virtual stack)

Lua 使用一个虚拟栈来和 C 传递值。 栈上的的每个元素都是一个 Lua 值 （nil，数字，字符串，等等）。

无论何时 Lua 调用 C，被调用的函数都得到一个新的栈， 这个栈独立于 C 函数本身的堆栈，也独立于以前的栈。 （译注：在 C 函数里，用 Lua API 不能访问到 Lua 状态机中本次调用之外的堆栈中的数据） 它里面包含了 Lua 传递给 C 函数的所有参数， 而 C 函数则把要返回的结果也放入堆栈以返回给调用者。

![](/images/Lua/=_UTF8_B_Li9RUeaIquWbvjIwMTYwODEwMTgxMTU1LnBuZw==_=.jpg)
方便起见，所有针对栈的 API 查询操作都不严格遵循栈的操作规则。 而是可以用一个索引来指向栈上的任何元素： `正的索引指的是栈上的绝对位置（从一开始）；负的索引则指从栈顶开始的偏移量`。 更详细的说明一下，如果堆栈有 n 个元素， 那么索引 1 表示第一个元素（也就是最先被压入堆栈的元素） 而索引 n 则指最后一个元素； 索引 -1 也是指最后一个元素（即栈顶的元素）， 索引 -n 是指第一个元素。 如果索引在 1 到栈顶之间（也就是，1 ≤ abs(index) ≤ top） 我们就说这是个有效的索引。

https://app.yinxiang.com/Home.action?login=true#n=bd8d9b9f-1094-40ad-9661-e5dfc4e7d214&s=s69&b=04e4ec26-42b3-4536-8e2a-54b2295bbe2b&ses=4&sh=1&sds=5&