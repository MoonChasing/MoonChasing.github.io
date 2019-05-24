---
title: cout从右向左入栈
date: 2018-12-18 21:04:49
tags: 坑
categories: 
  - [C++]
  - [编程]
---

之前一直读不懂书上说的 `cout` 运算顺序为从右向左什么意思。昨天遇到一个问题明白了一些。题意大概是让我们写个 `ID` 类，`ID` 类里有个方法是 `nextID()` ，即返回现在的 `id` ，再让 `id` 自增1（`id` 初始为0）。所以我觉得这样写就好。

```C++
int nextID()
{
    return id++;
}
```

在之后的输出中（为了简便，我把空格啥的都删了）

```C++
IDSystem x;
cout << x.nextID() << x.nextID() << x.nextID();
cout << x.nextID() << x.nextID() << x.nextID();
```

我本来觉得应该输出 0 1 2 3 4 5，但事实却是 2 1 0 5 4 3。

![mark](http://cmhblog.cfzhao.com/blog/20181218/9YfPs7CBO6CR.jpg)

我惊了，我本以为是 Codeblocks 出 Bug 了，重启后结果还是如此。此后，在校队群里问了后才想起知道原来是 `cout` 从右向左压栈搞的鬼。这样也就不难理解为什么如此了。(也懂了其他一些运算顺序从右向左的道理)

下面给出刁老板找出编译器汇编的代码：![mark](http://cmhblog.cfzhao.com/blog/20181218/bl9HYsHGn4zA.png)

原来 `cout` 是有栈的。
之后刁老板又测试了如下代码：![mark](http://cmhblog.cfzhao.com/blog/20181218/NK7N3KqhKHQJ.png)发现已经解释不通了。孙心乾大佬说一个语句中同时读和写同一个变量算是 ub(undefined behaviour) 吧。