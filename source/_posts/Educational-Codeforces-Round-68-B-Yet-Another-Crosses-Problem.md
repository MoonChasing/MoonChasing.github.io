---
title: Educational Codeforces Round 68 B Yet Another Crosses Problem
date: 2019-07-21 19:21:11
tags: 算法
---

## Problem

<div align="center">time limit per test : 2 seconds<br>memory limit per test : 256 megabytes<br>input : standard input<br>output : standard output</div>

### Description

You are given a picture consisting of  $nn$  rows and $m$ columns. Rows are numbered from  $1$  to  $n$  from the top to the bottom, columns are numbered from  $1$  to $m$ from the left to the right. Each cell is painted either black or white.

You think that this picture is not interesting enough. You consider a picture to be interesting if there is at least one *cross* in it. A cross is represented by a pair of numbers  $x$  and  $y$ , where  $1≤x≤n$  and $1≤y≤m$, such that **all cells** in row  $x$  and **all cells** in column  $y$ are painted black.

For examples, each of these pictures contain crosses:

![img](http://codeforces.com/predownloaded/2c/74/2c74594b8769dc2402be0ad1adc471acae3ae302.png)

The fourth picture contains 4 crosses: at  $(1,3)$ , (1,5)(1,5), $(3,3)$  and $(3,5)$ .

Following images don't contain crosses:

![img](http://codeforces.com/predownloaded/ec/a3/eca3d5d64cc48c29c29520d171af44c43d7cde6b.png)

You have a brush and a can of black paint, so you can make this picture interesting. Each minute you may choose a white cell and paint it black.

What is the minimum number of minutes you have to spend so the resulting picture contains at least one cross?

You are also asked to answer multiple independent queries.<!-- more -->

### Input

The first line contains an integer  $q$  (1≤q≤5⋅1041≤q≤5⋅104) — the number of queries.

The first line of each query contains two integers  $nn$  and $m$ (1≤n,m≤5⋅1041≤n,m≤5⋅104, n⋅m≤4⋅105n⋅m≤4⋅105) — the number of rows and the number of columns in the picture.

Each of the next  $nn$  lines contains $m$ characters — '.' if the cell is painted white and '*' if the cell is painted black.

It is guaranteed that $\sum n\le5\times10^4, \sum n\times m \le 4 \times 10^5$.



### Output

Print  $q$  lines, the $i$-th line should contain a single integer — the answer to the $i$-th query, which is the minimum number of minutes you have to spend so the resulting picture contains at least one cross.

### Example

#### input

```
9
5 5
..*..
..*..
*****
..*..
..*..
3 4
****
.*..
.*..
4 3
***
*..
*..
*..
5 5
*****
*.*.*
*****
..*.*
..***
1 4
****
5 5
.....
..*..
.***.
..*..
.....
5 3
...
.*.
.*.
***
.*.
3 3
.*.
*.*
.*.
4 4
*.**
....
*.**
*.**
```

#### output

```
0
0
0
0
0
4
1
1
2
```

Note

The example contains all the pictures from above in the same order.

The first 5 pictures already contain a cross, thus you don't have to paint anything.

You can paint  $(1,3)$ ,  $(3,1)$ ,  $(5,3)$ and $(3,5)$  on the 66-th picture to get a cross in $(3,3)$ . That'll take you 44 minutes.

You can paint  $ (1,2)$  on the $7$-th picture to get a cross in $(4,2)$ .

You can paint $(2,2)$ on the 88-th picture to get a cross in $(2,2)$. You can, for example, paint  $(1,3)$ ,  $(3,1)$  and $(3,3)$  to get a cross in $(3,3)$  but that will take you 33 minutes instead of  $1$ .

There are 9 possible crosses you can get in minimum time on the 99-th picture. One of them is in $ (1,1)$ : paint  $ (1,2)$  and  $(2,1)$ .

## Solution

对我来说毒瘤题目啊，B题，难受。一开始用的暴力的方法，wa在了2，一直找不到自己错在了哪里。眼看着别人一个一个过了这题，自己还一直卡在这，真的难受。几乎卡了整场比赛，最后没有时间看D题，赛后几乎秒出了Ｄ题。诶，还是太菜了，如果不卡这题的话应该能上很多分。

### Idea

分别统计出每行每组 '.' 的数量，最后遍历每个符号，用 `r[i] + r[j] - (maze[i][j] == '.')` 来更新最小值。

### Code

```C++
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
#define MAXN 400010
#define INF 0x3f3f3f3f
typedef long long LL;
 
using namespace std;
 
char maz[MAXN];
int T, n, m;
int main()
{
    int T;
    scanf("%d", &T);
    getchar();
    while(T--)
    {
        int r[50007] = {0};
        int c[50007] = {0};
        int ans = INF;
 
        scanf("%d%d", &n, &m);
        getchar();
        for(int i=1; i<=n; i++)
        {
            for(int j=1; j<=m; j++)
                maz[i*m+j] = getchar();
            getchar();
        }
 
        for(int i=1; i<=n; i++)
        {
            for(int j=1; j<=m; j++)
                if(maz[i*m+j] == '.')
                    r[i]++;
        }
 
        for(int j=1; j<=m; j++)
        {
            for(int i=1; i<=n; i++)
            {
                if(maz[i*m+j] == '.')
                    c[j]++;
            }
        }
 
        for(int i=1; i<=n; i++)
        {
            for(int j=1; j<=m; j++)
                ans = min(ans, r[i] + c[j] - (maz[i*m+j]=='.') );
        }
        printf("%d\n", ans);
    }
    return 0;
}
```

