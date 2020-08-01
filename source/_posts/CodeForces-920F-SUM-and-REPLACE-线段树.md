---
title: CodeForces 920F SUM and REPLACE(线段树)
date: 2018-08-03 10:07:52
tags: 线段树
categories: [ACM, 数据结构, 树状数组]
---

<h2 align="center">F. SUM and REPLACE </h2>

<div align="center">time limit per test: 2 seconds<br>memory limit per test: 256 megabytes<br>input: standard input<br>output: standard output</div>

## Description

Let *D*(*x*) be the number of positive divisors of a positive integer *x*. For example, *D*(2) = 2 (2 is divisible by 1 and 2), *D*(6) = 4 (6 is divisible by 1, 2, 3 and 6).<!--more-->

You are given an array *a* of *n* integers. You have to process two types of queries:

1. REPLACE *l* *r* — for every ![img](http://codeforces.com/predownloaded/33/4e/334e338c9804dd0f2ad09a404a1b0b0a64693034.png) replace *a**i* with *D*(*a**i*);
2. SUM *l* *r* — calculate ![img](http://codeforces.com/predownloaded/85/f3/85f3b14f57b1f05c219263ba36a0274dd8e82c2f.png).

Print the answer for each SUM query.

### Input

The first line contains two integers *n* and *m* (1 ≤ *n*, *m* ≤ 3·105) — the number of elements in the array and the number of queries to process, respectively.

The second line contains *n* integers *a*1, *a*2, ..., *a**n* (1 ≤ *a**i* ≤ 106) — the elements of the array.

Then *m* lines follow, each containing 3 integers *t**i*, *l**i*, *r**i* denoting *i*-th query. If *t**i* = 1, then *i*-th query is REPLACE *l**i* *r**i*, otherwise it's SUM *l**i**r**i* (1 ≤ *t**i* ≤ 2, 1 ≤ *l**i* ≤ *r**i* ≤ *n*).

There is at least one SUM query.

### Output

For each SUM query print the answer to it.

### Example

#### input

```
7 6
6 4 1 10 3 2 4
2 1 7
2 4 5
1 3 5
2 4 4
1 5 7
2 1 7
```

#### output

```
30
13
4
22
```

## Solution

### 题意

数组区间求和，区间更新为将 $a[i]$ 变为 $a[i]$ 的 因子个数。

### 思路

明显的线段树。

首先要暴力预处理出 $[1, 10^6]$ 每个数因子的个数。发现数字 1的因子数为1，2的因子数为2， 故 $num \le 2$ 时，不再会更新值。

~~这个题较坑的一点是会卡常数，我用`scanf()`输入会在Case2 T掉，而改用读入挂后只用了249ms。~~

### AC代码

```c++
#include <cstdio>
#include <cstring>
#include <cstdlib>
#include <iostream>
#include <set>
#include <map>
#include <queue>
#include <vector>
#include <utility>
#include <algorithm>
#define MAXN 1000005
#define INF 0x3f3f3f3f
#define DEBUG
#define DataIn
typedef long long LL;

using namespace std;

int n, m;
int divnum[MAXN];
int maxx[MAXN<<2];
LL sum[MAXN<<2];
int a[MAXN];

inline int read()
{
    int x=0,f=1;char ch=getchar();
    while(!isdigit(ch)) {if(ch=='-') f=-1;ch=getchar();}
    while(isdigit(ch)) {x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
void init()
{
    for(int i=1; i<MAXN; i++)
        for(int j=i; j<MAXN; j+=i)
            divnum[j]++;
}

void pushup(int root)
{
    sum[root] = sum[root<<1] + sum[root<<1|1];
    maxx[root] = max(maxx[root<<1], maxx[root<<1|1]);
}

void build(int root, int l, int r)
{
    if(l == r)
    {
        sum[root] = a[l];
        maxx[root] = a[l];
        return;
    }

    int mid = l+r>>1;
    build(root<<1, l, mid);
    build(root<<1|1, mid+1, r);
    pushup(root);
}

void update(int root, int l, int r, int a, int b)
{
    if(maxx[root] <= 2)//1的因子只有1， 2的因子有1、2两个，到此个数不再变化
        return;
    if(l == r)
    {
        maxx[root] = sum[root] = divnum[sum[root]];
        return;
    }

    int mid = l+r>>1;
    if(a<=mid)
        update(root<<1, l, mid, a, b);
    if(b>mid)
        update(root<<1|1, mid+1, r, a, b);
    pushup(root);
}

LL query(int root, int l, int r, int a, int b)
{
//    if(l>b || r<a)
//        return 0;
    if(a<=l && b>=r)
        return sum[root];
    int mid = l+r>>1;
    LL ans = 0;
    if(a <= mid)
        ans += query(root<<1, l, mid, a, b);
    if(b > mid)
        ans += query(root<<1|1, mid+1, r, a, b);
    return ans;
}

int main()
{
    init();

    n=read(), m=read();
    for(int i=1; i<=n; i++)
        a[i] = read();
    build(1, 1, n);
    int op, l ,r;
    for(int i=1; i<=m; i++)
    {
        op = read(), l = read(), r = read();
        if(op == 1)
            update(1, 1, n, l, r);
        else
            printf("%I64d\n", query(1, 1, n, l,r));
    }

    return 0;
}
```

![mark](http://cmhblog.cfzhao.com/blog/180803/i7abj3AJ1e.png)

| [41131343](http://codeforces.com/contest/920/submission/41131343) | 2018-08-02 20:37:41 | [MoonChasing](http://codeforces.com/profile/MoonChasing) | [F - SUM and REPLACE](http://codeforces.com/contest/920/problem/F) | GNU C++ | Accepted                      | 249 ms  | 54800 KB |
| ------------------------------------------------------------ | ------------------- | -------------------------------------------------------- | ------------------------------------------------------------ | ------- | ----------------------------- | ------- | -------- |
| [41131149](http://codeforces.com/contest/920/submission/41131149) | 2018-08-02 20:30:15 | [MoonChasing](http://codeforces.com/profile/MoonChasing) | [F - SUM and REPLACE](http://codeforces.com/contest/920/problem/F) | GNU C++ | Time limit exceeded on test 2 | 2000 ms | 54800 KB |
| [41130678](http://codeforces.com/contest/920/submission/41130678) | 2018-08-02 20:10:48 | [MoonChasing](http://codeforces.com/profile/MoonChasing) | [F - SUM and REPLACE](http://codeforces.com/contest/920/problem/F) | GNU C++ | Time limit exceeded on test 2 | 2000 ms | 54800 KB |

### 总结

~~第一次遇到卡常的题目，被读入挂惊呆了。没想到读入挂这么厉害。~~

更新：

这题自己会T并不是因为卡读入，而是因为一开始自己写的程序中，叶子结点时写的是 `l[::a]`，可能是因为::a双冒号访问会慢。将其改成`sum[root]`会可以AC。