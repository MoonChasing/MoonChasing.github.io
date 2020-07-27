---
title: C++ new失败时返回空指针
date: 2019-06-20 18:07:22
tags: [C++]
categories: [C++]
---

最近读《Effective C++》时读到了 new-handler 一章，了解了 new/delete 一些知识，才知道原来 new/delete 是可以个性化的。
先说简单的一个收获，C++ new失败后默认后抛出异常，如果我们想让他失败时返回空指针，则应该使用如下写法。

```c++
int *pi = new (std::nothrow) int[100000000000L];
```

