---
title: C++学习笔记
date: 2019-03-09 23:21:52
tags: C++, 学习笔记
categories: C++
---

String 字符串字符编码格式及其前缀

```c++
wchar_t title[] = L"Chief Astrogator";  // w_char string 宽字符
char16_t name[] = u"Felonia Ripova";   // char_16 string Unicode 16 
char32_t car[] = U"Humber Super Snipe";// char_32 string Unicode 32
```

UTF-8:  前缀u8

```C++
string a = u8"UTF_8 Encoding"; //UTF-8 char
```

Raw String: \不再转义， 前缀R, 使用()限界

```c++
cout << R"(Jim "King" Tutt uses "\n" instead of endl.)" << '\n';

cout << R"+*("(Who wouldn't?)", she whispered.)+*" << endl; //为了输出), 
//输出为"(Who wouldn't?)", she whispered.
```

<hr><!--more-->

C++ 11 结构体初始化可忽略 =

```C++
struct inflatable   // structure declaration
{char name[20];
 float volume;
 double price;
};

inflatable guest ={"Glorious Gloria",  // name value
                   1.88,               // volume value
                   29.99               // price value
                  };

inflatable duck {"Daphne", 0.12, 9.98};  // can omit the = in C++11
```

<hr>

```C++
class A
{
    private:
    	static double rate;
};
```

static 成员变量不属于对象的一部份，而是类别的一部份，所以程序可以在还没有诞生任何对象的时候就处理此种成员变量。但首先你必须初始化它。不要把static 成员变量的初始化动作安排在类别的构造式中，因为构造式可能一再被调用，而变量的初值却只应该设定一次。也不要把初始化动作安排在头文件中，因为它可能会被包含许多地方，因此也就可能被执行许多次。你应该在实作档中且类别以外的任何位置设定其初值。例如在 main 之中，或全域函数中，或任何函数之外：

```c++
double A::rate = 0.0075; // 设立static 成员变量的初值
```

这么做可曾考虑到m_rate 是个private 资料？没关系，设定static 成员变量初值时，不受任何存取权限的束缚。请注意，static 成员变量的型别也出现在初值设定句中，因为这是一个初值设定动作，不是一个数量指定（assignment）动作。事实上，static 成员变量是在这时候（而不是在类别声明中）才定义出来的。如果你没有做这个初始化动作，会产生联结错误：

> error LNK2001: unresolved external symbol "private: static double
> SavingAccount::m_rate"(?m_rate@SavingAccount@@2HA)

<p align="right">摘自候捷《深入浅出 MFC》</p>
<hr>

当用于内置类型的变量时，列表初始化有一个重要特点：如果我们使用列表初始化且初始值存在丢失信息的风险，则编译器将报错 


```C++
long double ld = 3.1415926536;
int a = {ld}; //error: 转换未执行，因为存在丢失信息的风险
```

<p align="right">——《C++ Primer 5e》P39</p>
<hr>

如果表达式的内容是解引用操作，则 `decltype` 将得到引用类型。


```C++
int i=42, *p=&i;
decltype(*p) b; //error: decltype(*p) declared as reference 
				//but not initialized

```

如果  `decltype` 使用的是一个不加括号的变量，则得到的结果就是该变量的类型；如果给变量加上了一层或多层括号，编译器就会把它当成是一个表达式。变量是一种可以作为赋值语句左传的特殊表达式，所以这样的 `decltype` 就会得到引用类型。

```C++
int i = 42;
decltype((i)) a; //error: a 是 int&，必须初始化
decltype(i) b; //ok: e 是 一个未初始化的 int
```

切记： `decltype((variable))` （注意是双层括号）的结果永远是引用， 而 `decltype(variable)` 的结果只有当 variable 本身就是一个引用时才是引用。

<p align="right">——《C++ Primer 5e》P63</p><hr>
当一个算术表达式中既有 `int` 又有无符号数时，那个 `int` 值就会转换成无符号数。

如果一条表达式中已经有了 `size()` 函数就不要再使用 `int` 了， 这样可以避免混用 `int` 和 `unsigned` 可能带来的问题。

<hr>

当把 `string` 对象和字符字面值及字符串字面值混在一条语句中使用时， 必须确保每个加法运算符的两侧的运算对象至少有一个是 `string`。

<hr>
如果容器为空，则 `begin` 和 `end` 返回的是同一个迭代器，都是尾后迭代器。[^note]

[^note]: 测试 markdown 注释语法。

<hr>

```C++
int ia[] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
decltype(ia) p;
```

p 的类型是由 10 个整数构成的数组；

<hr>

给指针加上一个整数，得到的新指针仍需指向同一数组的其他元素，或者指向同一数组的尾元素的下一位置

```C++
constexpr size_t sz = 5;
int arr[sz] = {1, 2, 3, 4, 5};
int *p = arr+sz;  //使用警告：不能解引用
int *p2 = arr+10; //错误
```

<hr>

当一个对象被用作右值时，用的是对象的值（内容）；当对象被用作左值时，用的是对象的身份（在内存中的位置）。

<hr>








