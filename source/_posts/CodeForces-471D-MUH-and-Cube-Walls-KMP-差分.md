---
title: CodeForces 471D MUH and Cube Walls(KMP+差分)
date: 2018-08-03 14:00:59
tags:
categories: [ACM, 字符串, KMP]
---

<h2 align="center">D. MUH and Cube Walls </h2>

<div align="center">time limit per test: 2 seconds<br>memory limit per test: 256 megabytes <br>input: standard input<br>output: standard output</div>

## Description

Polar bears Menshykov and Uslada from the zoo of St. Petersburg and elephant Horace from the zoo of Kiev got hold of lots of wooden cubes somewhere. They started making cube towers by placing the cubes one on top of the other. They defined multiple towers standing in a line as a wall. A wall can consist of towers of different heights.<!--more-->

Horace was the first to finish making his wall. He called his wall an elephant. The wall consists of *w* towers. The bears also finished making their wall but they didn't give it a name. Their wall consists of *n* towers. Horace looked at the bears' tower and wondered: in how many parts of the wall can he "see an elephant"? He can "see an elephant" on a segment of *w* contiguous towers if the heights of the towers on the segment match as a sequence the heights of the towers in Horace's wall. In order to see as many elephants as possible, Horace can raise and lower his wall. He even can lower the wall below the ground level (see the pictures to the samples for clarification).

Your task is to count the number of segments where Horace can "see an elephant".

### Input

The first line contains two integers *n* and *w* (1 ≤ *n*, *w* ≤ 2·105) — the number of towers in the bears' and the elephant's walls correspondingly. The second line contains *n* integers *a**i* (1 ≤ *a**i* ≤ 109) — the heights of the towers in the bears' wall. The third line contains *w* integers *b**i* (1 ≤ *b**i* ≤ 109) — the heights of the towers in the elephant's wall.

### Output

Print the number of segments in the bears' wall where Horace can "see an elephant".

### Examples

#### input

```
13 5
2 4 5 5 4 3 2 2 2 3 3 2 1
3 4 4 3 2
```

#### output

```
2
```

#### Note

The picture to the left shows Horace's wall from the sample, the picture to the right shows the bears' wall. The segments where Horace can "see an elephant" are in gray.

![mark](http://cmhblog.cfzhao.com/blog/180803/3jljc9If7d.png)

## Solution

### 思路

首先差分，计算出主串和模式串中相邻数字的差分数组。(去掉首位的)

之后就是KMP匹配这两个差分数组了了。

### AC代码

```
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
#define MAXN 200005
#define INF 0x3f3f3f3f
#define DEBUG
#define DataIn
typedef long long LL;

using namespace std;

inline int read()
{
    int x=0,f=1;char ch=getchar();
    while(!isdigit(ch)) {if(ch=='-') f=-1;ch=getchar();}
    while(isdigit(ch)) {x=x*10+ch-'0';ch=getchar();}
    return x*f;
}

int n, w;
int a[MAXN], b[MAXN];
int next[MAXN];

void prekmp()
{
    int i=0, j=-1;
    next[0] = -1;
    while(i<w)
    {
        while(~j && b[i]!=b[j])
            j = next[j];
        next[++i] = ++j;
    }
}

int kmpcnt()
{
    int i=0, j=0;
    int cnt = 0;
    prekmp();
    while(i<n)
    {
        while(~j && a[i]!=b[j])
            j = next[j];
        i++; j++;
        if(j >= w)
        {
            cnt++;
            j = next[j];
        }
    }
    return cnt;
}

int main()
{
    n = read(), w = read();
    for(int i=0; i<n; i++)
        a[i] = read();
    for(int i=n-1; i>0; i--)
        a[i] -= a[i-1];
    for(int i=0; i<n-1; i++)
        a[i] = a[i+1];
//    for(int i=0; i<n-1; i++)
//        printf("%d ", a[i])
//    putchar(10);
    n--;

    for(int i=0; i<w; i++)
        b[i] = read();
    for(int i=w-1; i>0; i--)
        b[i] -= b[i-1];
    for(int i=0; i<w-1; i++)
        b[i] = b[i+1];
    w--;
    if(w==0)//子中长度为1时要特判
        printf("%d\n", n+1);
    else
        printf("%d\n", kmpcnt());
    return 0;
}
```

![mark](http://cmhblog.cfzhao.com/blog/180803/9JEEEBaKGF.png)

| #                                                            | When                | Who                                                      | Problem                                                      | Lang    | Verdict                | Time  | Memor   |
| ------------------------------------------------------------ | ------------------- | -------------------------------------------------------- | ------------------------------------------------------------ | ------- | ---------------------- | ----- | ------- |
| [41132718](http://codeforces.com/contest/471/submission/41132718) | 2018-08-02 21:26:50 | [MoonChasing](http://codeforces.com/profile/MoonChasing) | [D - MUH and Cube Walls](http://codeforces.com/contest/471/problem/D) | GNU C++ | Accepted               | 31 ms | 2400 KB |
| [41132612](http://codeforces.com/contest/471/submission/41132612) | 2018-08-02 21:23:05 | [MoonChasing](http://codeforces.com/profile/MoonChasing) | [D - MUH and Cube Walls](http://codeforces.com/contest/471/problem/D) | GNU C++ | Wrong answer on test 2 | 30 ms | 2400 KB |