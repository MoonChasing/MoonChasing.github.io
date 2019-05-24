---
title: CodeForces 911D Inversion Counting(思维)
date: 2018-08-03 10:27:02
tags:
categories: [ACM, 思维]
---

<h2 align="center">D. Inversion Counting  </h2>

<div align="center">time limit per test: 2 seconds<br>memory limit per test: 256 megabytes<br>input: standard input<br>output: standard output</div>

## Description

A permutation of size *n* is an array of size *n* such that each integer from 1 to *n* occurs exactly once in this array. An inversion in a permutation *p* is a pair of indices (*i*, *j*) such that *i* > *j* and *a**i* < *a**j*. For example, a permutation [4, 1, 3, 2] contains 4 inversions: (2, 1), (3, 1), (4, 1), (4, 3).<!--more-->

You are given a permutation *a* of size *n* and *m* queries to it. Each query is represented by two indices *l* and *r* denoting that you have to reverse the segment [*l*, *r*] of the permutation. For example, if *a* = [1, 2, 3, 4] and a query *l* = 2, *r* = 4 is applied, then the resulting permutation is [1, 4, 3, 2].

After each query you have to determine whether the number of inversions is odd or even.

### Input

The first line contains one integer *n* (1 ≤ *n* ≤ 1500) — the size of the permutation.

The second line contains *n* integers *a*1, *a*2, ..., *a**n* (1 ≤ *a**i* ≤ *n*) — the elements of the permutation. These integers are pairwise distinct.

The third line contains one integer *m* (1 ≤ *m* ≤ 2·105) — the number of queries to process.

Then *m* lines follow, *i*-th line containing two integers *l**i*, *r**i* (1 ≤ *l**i* ≤ *r**i* ≤ *n*) denoting that *i*-th query is to reverse a segment [*l**i*, *r**i*] of the permutation. All queries are performed one after another.

### Output

Print *m* lines. *i*-th of them must be equal to odd if the number of inversions in the permutation after *i*-th query is odd, and even otherwise.

### Examples

#### input

```
3
1 2 3
2
1 2
2 3
```

#### output

```
odd
even
```

#### input

```
4
1 2 4 3
4
1 1
1 4
1 4
2 3

```

#### output

```
odd
odd
odd
even
```

## Solution

### 题意

给你一个数组和若干区间，问你区间翻转后数组中逆序对数为奇数还是偶数。

### 思路

思维题目：首先求出区间的长度 $len = r-l+1$ ，那么区间内共有 $tot = \frac {len * (len-1)} 2$ 个数对。若 $tot$ 为奇数，则不管该区间有多少个逆序对，翻转区间后逆序对数奇偶性改变； 若为偶数，则不变。

所以我们可以先暴力求出整体逆序对数，再对区间进行以上处理。

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
#define MAXN 1505
#define INF 0x3f3f3f3f
#define DEBUG
#define DataIn
typedef long long LL;

using namespace std;

int a[MAXN];
int n, m;
int main()
{
    int all = 0;
    scanf("%d", &n);
    for(int i=0; i<n; i++)
        scanf("%d", a+i);
    for(int i=0; i<n-1; i++)
    {
        for(int j=i+1; j<n; j++)
            if(a[i] > a[j])
                all++;
    }

    bool isodd = false;
    if(all & 1)
        isodd = true;

    int l, r, len;
    scanf("%d", &m);
    for(int i=0; i<m; i++)
    {
        scanf("%d%d", &l, &r);
        len = r-l+1;
        if((len * (len-1) / 2) & 1)
            isodd = !isodd;
        if(isodd)
            puts("odd");
        else
            puts("even");
    }
    return 0;
}
```

![mark](http://cmhblog.cfzhao.com/blog/180803/c8k5Eg98CH.png)

|                              #                               |        When         | Who                                                      | Problem                                                      | Lang    | Verdict  | Time  | Memory |
| :----------------------------------------------------------: | :-----------------: | -------------------------------------------------------- | ------------------------------------------------------------ | ------- | -------- | :---: | :----: |
| [41129808](http://codeforces.com/problemset/submission/911/41129808) | 2018-08-02 19:31:53 | [MoonChasing](http://codeforces.com/profile/MoonChasing) | [911D - Inversion Counting](http://codeforces.com/problemset/problem/911/D) | GNU C++ | Accepted | 93 ms |  0 KB  |

