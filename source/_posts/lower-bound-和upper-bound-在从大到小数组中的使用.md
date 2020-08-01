---
title: lower_bound()和upper_bound()在从大到小数组中的使用
date: 2018-08-11 11:04:03
tags:
categories: [C++, STL]
---

今天突发奇想，想知道 `lower_bound()` 和 `upper_bound()` 函数在从大到小的数组中怎么使用，猜想 C++ 中应该会有重载， 果不其然， 正确打开姿势是酱紫。

```c++
lower_bound(begin, end, val, greater<T>())
```

