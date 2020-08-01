---
title: POJ 1426 Find The Multiple(BFS)
date: 2018-08-15 15:39:22
tags: kuangbin带你飞
categories: [ACM, BFS]
---

传送门：[Find the Multiple](http://poj.org/problem?id=1426)

<blockquote class="blockquote-center">人一我百！人十我万！永不放弃~~~ 怀着自信的心，去追逐梦想 ——kuangbin </blockquote>

## Problem

### Description

Given a positive integer n, write a program to find out a nonzero multiple m of n whose decimal representation contains only the digits 0 and 1. You may assume that n is not greater than 200 and there is a corresponding m containing no more than 100 decimal digits.<!--more-->

### Input

The input file may contain multiple test cases. Each line contains a value of n (1 <= n <= 200). A line containing a zero terminates the input.

### Output

For each value of n in the input print a line containing the corresponding value of m. The decimal representation of m must not contain more than 100 digits. If there are multiple solutions for a given value of n, any one of them is acceptable.

### Sample Input

```
2
6
19
0
```

### Sample Output

```
10
100100100100100100
111111111111111111
```

## Solution

### 题意

只用10构成一个数，要求长度不能超过200，此数是所给数的倍数。

### 思路

这题可能数据水，可能有别的规律，用LL + BFS就水过去了。不知其所以然。

### AC代码

```c++
LL tmp;
LL bfs(int n)
{
    queue<LL> que;
    que.push(1);
    while(!que.empty())
    {
        tmp = que.front();
        que.pop();
        if(tmp % n == 0)
            return tmp;
        que.push(tmp*10);
        que.push(tmp*10+1);
    }
    return 0;
}
int main()
{
    int n;
    LL ans;
    while(scanf("%d", &n) && n)
    {
        ans = bfs(n);
        printf("%lld\n", ans);
    }
    return 0;
}
```

