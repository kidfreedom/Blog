---
title: C++拷贝构造函数和赋值运算符
top: false
cover: false
toc: true
mathjax: true
date: 2020-06-22 23:19:22
password:
summary:
tags: 
- C++
- 面试
categories: 
- 语言基础
---

## 定义
在默认情况下（用户没有定义，但是也没有显式的删除），编译器会自动的隐式生成一个拷贝构造函数和赋值运算符。但用户可以使用`delete`来指定不生成拷贝构造函数和赋值运算符，这样的对象就不能通过值传递，也不能进行赋值运算。

显示定义：
```cpp
class Preson
{
public:
    // 拷贝构造函数
    Preson(const Preson& other)
    {

    }

    // 赋值运算符函数
    Preson& operator=(const Preson& other)
    {

    }
}
```

C++11新特性，显示删除：
```cpp
class Preson
{
public:
    // 拷贝构造函数
    Preson(const Preson& other) = delete;

    // 赋值运算符函数
    Preson& operator=(const Preson& other) = delete;
}
```

> 注意：拷贝构造函数的参数一定是以`引用的方式传递的`，如果使用值传递的方式，强出现无限循环递归，栈会溢出。

## 何时调用
```cpp
#include <iostream>
using namespace std;

class Preson
{
public:
	Preson() {}

	Preson(const Preson& other)
	{
		cout << "拷贝构造函数" << endl;

		name = other.name;
	}

	Preson& operator=(const Preson& other)
	{
		cout << "赋值运算符函数" << endl;

		name = other.name;
		return *this;
	}

private:
	string name;
};

void Fun1(Preson p)
{

}

Preson Fun2()
{
	Preson p;
	return p;
}

int main()
{
	Preson p;
	cout << "1" << endl;
	Preson p1 = p;

	Preson p2;
	cout << "2" << endl;
	p2 = p;

	cout << "3" << endl;
	Fun1(p);

	cout << "4" << endl;
	p2 = Fun2();

	cout << "5" << endl;
	Preson p3 = Fun2();
}
```

运行结果如下：
![](/images/C++拷贝构造函数和赋值运算符/1.jpg)

1. 虽然出现的`=`，但是实际上使用对象p来创建一个新的对象p1，也就是构造新的对象，所以调用的是`拷贝构造函数`。
2. 首先声明一个对象p2，然后使用赋值运算符"="，将p的值复制给p2，显然是调用赋值运算符，为一个已经存在的对象赋值，所以调用的是`赋值运算符函数`。
3. 以`值传递的方式`将对象p2传入函数Fun1内，调用拷贝构造函数`构建一个函数Fun1可用的实参`，所以调用的是`拷贝构造函数`。
4. 在函数Fun2以值传递的方式返回时，用拷贝构造函数创建一个临时对象作为返回值，，所以调用的是`拷贝构造函数`。在函数返回后将临时变量赋值给已经初始化过的p2，所以调用的是`赋值运算符函数`。
5. 如果按照4的情况解释，应该是首先调用拷贝构造函数创建临时对象；然后再调用拷贝构造函数使用刚才创建的临时对象创建新的对象p3，也就是会调用两次拷贝构造函数。可能是编译器优化了，应该是直接调用拷贝构造函数使用返回值创建了对象p3，所以只调用了一次`拷贝构造函数`。

### 拷贝构造函数
1. 当对象作为函数的参数，以值传递的方式传递给函数时。
2. 当对象作为函数的返回值，以值传递的方式返回时。
3. 当使用一个对象初始化另一个对象时。

### 赋值运算符函数
当一个对象赋值给另一个已经初始化完成的对象时。

## 什么时候需要使用
