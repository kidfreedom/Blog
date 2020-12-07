---
title: lua_next用法
top: false
cover: false
toc: true
mathjax: true
date: 2020-12-05 16:49:02
password:
summary:
tags: 
- Lua C API 
categories: 程序语言
---

# lua_next用法

```cpp
int lua_next (lua_State *L, int index);
```

1. 先从栈顶弹出一个key
2. 从栈指定位置的 table 里取相对于刚刚弹出的key的下一对key-value，先将key入栈再将 value入栈
3. 如果第2步成功则返回非0值，否则返回0，并且不向栈中压入任何值。table里第一对key-value的前面没有数据，所以先用 lua_pushnil() 压入一个 nil 充当初始 key。如果想从特定位置开始，须先将开始位置前一个key压入栈，再调用lua_next()

-----------------------------------

## 一维表的遍历

Lua：
```lua
local array = {'a', 'b', 'c'}
```

Cpp:
```cpp
void traversalLuaTable(lua_State *L)
{
   //将全局变量array压入栈
   lua_getglobal(L, "array"); 
   //table 里第一对 key-value 的前面没有数据，所以先用 lua_pushnil()压入一个 nil 始 key。
   lua_pushnil(L);
   //此时栈顶为nil,往下才是array,故index为-2
   while (lua_next(L, -2) != 0)
   {
        //因为key先入栈value后入栈,故以下index为-1
        //获取value值类型,并根据类型获取值,打印
        auto valueType = lua_type(L, -1);
        std::cout << "valueType : " << lua_typename(L, valueType) << std::endl;
        switch (valueType)
        {
        case LUA_TNUMBER:
            std::cout << "value : " << lua_tonumber(L, -1) << std::endl;
            break;
        case LUA_TSTRING:
            std::cout << "value : " << lua_tostring(L, -1) << std::endl;
            break;
        default:
            break;
        }

        //因为key先入栈value后入栈,故以下index为-2
        //获取key值类型,并根据类型获取值,打印
        auto keyType = lua_type(L, -2);
        std::cout << "keyType : " << lua_typename(L, keyType) << std::endl;
        switch (keyType)
        {
        case LUA_TNUMBER:
            std::cout << "key : " << lua_tonumber(L, -2) << std::endl;
            break;
        case LUA_TSTRING:
            std::cout << "key : " << lua_tostring(L, -2) << std::endl;
            break;
        default:
            break;
        }
        //这一步尤为重要
        //弹出value，让key留在栈顶
        //因为lua_next会先弹出前一个key,在判断table中还有没有key-value,故此处只弹出ue就可以了
        lua_pop(L, 1);
    }
}
```

输出结果:
```console
valueType : string
value : a
keyType : number
key : 1
valueType : string
value : b
keyType : number
key : 2
valueType : string
value : c
keyType : number
key : 3
```

-------------------------------------

## 二维tabel遍历

Lua:
```lua
local array1 = {'a', 'b', 'c'}
local array2 = {'d', 'e', 'f'}
local list = {array1, array2}
```

Cpp:
```cpp
void traversalLuaTable(lua_State *L)
{
   lua_getglobal(L, "list");
   lua_pushnil(L);
   while (lua_next(L, -2) != 0)
   {
       if (lua_istable(L, -1))
       {
            lua_pushnil(L);
            while (lua_next(L, -2) != 0)
            {
                auto valueType = lua_type(L, -1);
                std::cout << "valueType : " << lua_typename(L, valueType) << ::endl;
                switch (valueType)
                {
                case LUA_TNUMBER:
                    std::cout << "value : " << lua_tonumber(L, -1) << ::endl;
                    break;
                case LUA_TSTRING:
                    std::cout << "value : " << lua_tostring(L, -1) << ::endl;
                    break;
                default:
                    break;
                }

                auto keyType = lua_type(L, -2);
                std::cout << "keyType : " << lua_typename(L, keyType) << ::endl;
                switch (keyType)
                {
                case LUA_TNUMBER:
                    std::cout << "key : " << lua_tonumber(L, -2) << std::endl;
                    break;
                case LUA_TSTRING:
                    std::cout << "key : " << lua_tostring(L, -2) << std::endl;
                    break;
                default:
                    break;
                }
                lua_pop(L, 1);
            }
        }
        lua_pop(L, 1);
    }
}
```

输出结果:
```console
valueType : string
value : a
keyType : number
key : 1
valueType : string
value : b
keyType : number
key : 2
valueType : string
value : c
keyType : number
key : 3
valueType : string
value : d
keyType : number
key : 1
valueType : string
value : e
keyType : number
key : 2
valueType : string
value : f
keyType : number
key : 3
```