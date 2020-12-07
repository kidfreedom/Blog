---
title: lua_settable和lua_rawset用法
top: false
cover: false
toc: true
mathjax: true
date: 2020-12-05 17:18:51
password:
summary:
tags: 
- Lua C API 
categories: 程序语言
---

## lua_settable

```cpp
void lua_settable (lua_State *L, int index);
```

等价于t[k] = v的操作， 这里t是一个给定有效索引index处的值， v指栈顶的值，而k是栈顶之下的那个值。

这个函数会把键和值都从堆栈中弹出。和在 Lua 中一样，这个函数可能触发 “newindex” 事件的元方法 。

其实这个解释的意思就是，lua_settable 会把栈顶作为value,栈顶的下一个作为key设置到index指向的table，最后把这两个弹出弹出栈，这时候settable完成。

## lua_rawset
```cpp
void lua_rawset (lua_State *L, int index);
```

类似于 lua_settable， 但是是作一个直接赋值（不触发元方法）。
用法同lua_settable,但lua_rawset更快(因为当key不存在时不用访问元方法__newindex

Lua:
```lua 
local array = {'a', 'b', 'c'}
modifyLuaArray()
for key, var in ipairs(array) do
    print('%s', var)
end
```

Cpp:
```cpp
void modifyLuaArray(lua_State *L)
{
   //将全局变量arrayyaruzhan
   lua_getglobal(L, "array"); 
   //获取array长度,因为array在栈顶,所以索引为-1,此代码等同于#array
   int n = lua_objlen(L, -1); 
   for (int i = 1; i <= n; ++i)
   {
       //将要查找的key压入栈中,因为table索引是从1开始
        lua_pushnumber(L, i);
        //将i弹出并将array[i]放在栈顶
        //此处可以用lua_settable代替,但效率更低
        lua_rawget(L, -2);
        //取得value
        std::string s = lua_tostring(L, -1);
        //弹出value
        lua_pop(L, 1);
        //修改字符串
        s += "_";
        //压入key
        lua_pushnumber(L, i);
        //压入value
        lua_pushstring(L, s.c_str());
        //设置在-3位置的table,key为索引,value为值,即table[key] = value
        //因为栈顶为value,往下一个是key,故此处索引为-3
        //此处可以用lua_settable代替,但效率更低
        lua_rawset(L, -3);
    }
}
```

输出结果:
```console
a
b
c
```