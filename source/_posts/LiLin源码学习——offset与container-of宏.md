---
title: Linux源码学习——offsetof与container_of宏
date: 2019-07-18 23:29:59
tags: [Linux, Linux源码学习]
categories: [Linux, Linux源码学习]
---

<blockquote class="blockquote-center">Linux内核源码果真精髓，相见恨晚。</blockquote>

`offsetof` 宏定义如下

```C
#define offsetof(type, member) (size_t)&(((type*)0)->member)
```

`container_of` 宏定义如下

```C
#define container_of(ptr, type, member) ({ \
     const typeof( ((type *)0)->member ) *__mptr = (ptr); \
     (type *)( (char *)__mptr - offsetof(type,member) );}) 
```

