---
title: '《Effective C++》读书笔记——条款31:将文件间的编译依存关系降至最低'
date: 2019-04-30 15:07:20
tags: [C++, 读书笔记]
categories: C++
---

读 《Effective C++》条款 31 的时候一直模模糊糊的，因为不了解 C++ 编译依存的原理，不知道什么时候会重新编译，什么时候不会。所以读的时候很是纠结，不明白该条款的意义。后来读了这篇文章，就清晰了很多。博主用一个简短的程序例子，展示了 C++ 基本的编译依存原则。感谢。

读完条款 31 ，我也更加深刻的明白了 C 语言为什么头文件和实现文件分开。大一时老师说的结构更加清晰或许是更次要的作用。其主要作用是降低文件间的编译依存关系，加快编译速度。

下面是我读的文章，[原文链接](http://www.cnblogs.com/jerry19880126/p/3551836.html)

在说这一条款之前，先要了解一下C/C++的编译知识，假设有三个类ComplexClass, SimpleClass1和SimpleClass2，采用头文件将类的声明与类的实现分开，这样共对应于6个文件，分别是ComplexClass.h，ComplexClass.cpp，SimpleClass1.h，SimpleClass1.cpp，SimpleClass2.h，SimpleClass2.cpp。

ComplexClass复合两个BaseClass，SimpleClass1与SimpleClass2之间是独立的，ComplexClass的.h是这样写的：


```
#ifndef COMPLESS_CLASS_H
#define COMPLESS_CLASS_H

#include “SimpleClass1.h”
#include “SimpleClass2.h”

class ComplexClass
{
    SimpleClass1 xx;
    SimpleClass2 xxx;
};
…

#endif /* COMPLESS _CLASS_H */
```


我们来考虑以下几种情况：

**Case 1：**

现在SimpleClass1.h发生了变化，比如添加了一个新的成员变量，那么没有疑问，SimpleClass1.cpp要重编，SimpleClass2因为与SimpleClass1是独立的，所以SimpleClass2是不需要重编的。

 

那么现在的问题是，ComplexClass需要重编吗？

 

答案是“是”，因为ComplexClass的头文件里面包含了SimpleClass1.h（使用了SimpleClass1作为成员对象的类），**而且所有使用****ComplexClass****类的对象的文件，都需要重新编译！**

 

如果把ComplexClass里面的#include “SimpleClass1.h”给去掉，当然就不会重编ComplexClass了，但问题是也不能通过编译了，因为ComplexClass里面声明了SimpleClass1的对象xx。那如果把#include “SimpleClass1.h”换成类的声明class SimpleClass1，会怎么样呢？能通过编译吗？

 

答案是“否”，因为编译器需要知道ComplexClass成员变量SimpleClass1对象的大小，而这些信息仅由class SimpleClass1是不够的，但如果SimpleClass1作为一个函数的形参，或者是函数返回值，用class SimpleClass1声明就够了。如：

```
1 // ComplexClass.h
2 class SimpleClass1;
3 …
4 SimpleClass1 GetSimpleClass1() const;
5 …
```

但如果换成指针呢？像这样：


```
 1 // ComplexClass.h
 2 #include “SimpleClass2.h”
 3 
 4 class SimpleClass1;
 5 
 6 class ComplexClass: 
 7 {
 8     SimpleClass1* xx;
 9     SimpleClass2 xxx;
10 };
```


这样能通过编译吗？

 

答案是“是”，因为编译器视所有指针为一个字长（在32位机器上是4字节），因此class SimpleClass1的声明是够用了。但如果要想使用SimpleClass1的方法，还是要包含SimpleClass1.h，但那是ComplexClass.cpp做的，因为ComplexClass.h只负责类变量和方法的声明。

 

那么还有一个问题，如果使用SimpleClass1*代替SimpleClass1后，SimpleClass1.h变了，ComplexClass需要重编吗？

先看Case2。

 

***Case 2：***

回到最初的假定上（成员变量不是指针），现在SimpleClass1.cpp发生了变化，比如改变了一个成员函数的实现逻辑（换了一种排序算法等），但SimpleClass1.h没有变，那么SimpleClass1一定会重编，SimpleClass2因为独立性不需要重编，那么现在的问题是，ComplexClass需要重编吗？

 

答案是“否”，因为编译器重编的条件是发现一个变量的类型或者大小跟之前的不一样了，但现在SimpleClass1的接口并没有任务变化，只是改变了实现的细节，所以编译器不会重编。

 

***Case 3：***

结合Case1和Case2，现在我们来看看下面的做法：

```
 1 // ComplexClass.h
 2 #include “SimpleClass2.h”
 3 
 4 class SimpleClass1;
 5 
 6 class ComplexClass
 7 {
 8     SimpleClass1* xx;
 9     SimpleClass2 xxx;
10 };
```

```
1 // ComplexClass.cpp
2 
3 void ComplexClass::Fun()
4 {
5     SimpleClass1->FunMethod();
6 }
```


请问上面的ComplexClass.cpp能通过编译吗？

 

答案是“否”，因为这里用到了SimpleClass1的具体的方法，所以需要包含SimpleClass1的头文件，但这个包含的行为已经从ComplexClass里面拿掉了（换成了class SimpleClass1），所以不能通过编译。

 

如果解决这个问题呢？其实很简单，只要在ComplexClass.cpp里面加上#include “SimpleClass1.h”就可以了。换言之，我们其实做的就是将ComplexClass.h的#include “SimpleClass1.h”移至了ComplexClass1.cpp里面，而在原位置放置class SimpleClass1。

 

这样做是为了什么？假设这时候SimpleClass1.h发生了变化，会有怎样的结果呢？

SimpleClass1自身一定会重编，SimpleClass2当然还是不用重编的，ComplexClass.cpp因为包含了SimpleClass1.h，所以需要重编，但换来的好处就是**所有用到ComplexClass的其他地方，它们所在的文件不用重编了**！因为ComplexClass的头文件没有变化，接口没有改变！

 

总结一下，对于C++类而言，如果它的头文件变了，那么所有这个类的对象所在的文件都要重编，但如果它的实现文件（cpp文件）变了，而头文件没有变（对外的接口不变），那么所有这个类的对象所在的文件都不会因之而重编。

因此，避免大量依赖性编译的解决方案就是：**在头文件中用class声明外来类，用指针或引用代替变量的声明；在cpp文件中包含外来类的头文件。**