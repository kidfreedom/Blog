---
title: lua_gettable和lua_rawget用法
top: false
cover: false
toc: true
mathjax: true
date: 2020-12-05 17:11:04
password:
summary:
tags: Lua C API 
categories:
- 程序语言
- Lua
---

## lua_gettable

```cpp
void lua_gettable (lua_State *L, int index);
```

把t[k]值压入堆栈， 这里的t是指有效索引index指向的值，而k则是栈顶放的值。

这个函数会弹出堆栈上的key（把结果放在栈上相同位置）。在Lua中，这个函数可能触发对应`index`事件的元方法 。

------------------------

Lua:
```lua
local array = {'a', 'b', 'c'}
```

Cpp:
```cpp
void readLuaArray(lua_State *L)
{
    //获取全局变量array
    lua_getglobal(L, "array");
    //获取array长度,因为array在栈顶,所以索引为-1,此代码等同于#array
    int n = lua_objlen(L, -1);
    for (int i = 1; i <= n; ++i)
    {
        //将要查找的key压入栈中,因为table索引是从1开始
        lua_pushnumber(L, i);
        //将栈顶元素作为key,查找索引位置的tabel的值,并把key弹出压入value
        //此处栈顶为刚压入的i,往下一个才是array,故索引为-2
        //此处可以用lua_rawget代替,因为已知array是个数组型table,故可以跳过元方法,这样效率更快
        lua_gettable(L, -2);
        //将栈顶元素转为char*
        cout<<lua_tostring(L, -1)<<endl;
        //弹出栈顶元素value,以便下一轮循环,array依然在栈顶
        lua_pop(L, 1);
    }
}
```

输出结果:
```console
a
b
c
```