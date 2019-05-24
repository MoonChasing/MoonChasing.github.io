---
title: CodeForces 731C Socks(并查集)
date: 2018-08-03 10:47:02
tags:
categories: ACM
---

<h2 align="center">C. Socks  </h2>

<div align="center">time limit per test: 2 seconds<br>memory limit per test: 256 megabytes<br>input: standard input<br>output: standard output</div>

## Description

Arseniy is already grown-up and independent. His mother decided to leave him alone for *m* days and left on a vacation. She have prepared a lot of food, left some money and washed all Arseniy's clothes.<!--more-->

Ten minutes before her leave she realized that it would be also useful to prepare instruction of which particular clothes to wear on each of the days she will be absent. Arseniy's family is a bit weird so all the clothes is enumerated. For example, each of Arseniy's *n* socks is assigned a unique integer from 1 to *n*. Thus, the only thing his mother had to do was to write down two integers *l**i* and *r**i* for each of the days — the indices of socks to wear on the day *i* (obviously, *l**i* stands for the left foot and *r**i* for the right). Each sock is painted in one of *k*colors.

When mother already left Arseniy noticed that according to instruction he would wear the socks of different colors on some days. Of course, that is a terrible mistake cause by a rush. Arseniy is a smart boy, and, by some magical coincidence, he posses *k* jars with the paint — one for each of *k* colors.

Arseniy wants to repaint some of the socks in such a way, that for each of *m* days he can follow the mother's instructions and wear the socks of the same color. As he is going to be very busy these days he will have no time to change the colors of any socks so he has to finalize the colors now.

The new computer game Bota-3 was just realised and Arseniy can't wait to play it. What is the minimum number of socks that need their color to be changed in order to make it possible to follow mother's instructions and wear the socks of the same color during each of *m* days.

### Input

The first line of input contains three integers *n*, *m* and *k* (2 ≤ *n* ≤ 200 000, 0 ≤ *m* ≤ 200 000, 1 ≤ *k* ≤ 200 000) — the number of socks, the number of days and the number of available colors respectively.

The second line contain *n* integers *c*1, *c*2, ..., *c**n* (1 ≤ *c**i* ≤ *k*) — current colors of Arseniy's socks.

Each of the following *m* lines contains two integers *l**i* and *r**i* (1 ≤ *l**i*, *r**i* ≤ *n*, *l**i* ≠ *r**i*) — indices of socks which Arseniy should wear during the *i*-th day.

### Output

Print one integer — the minimum number of socks that should have their colors changed in order to be able to obey the instructions and not make people laugh from watching the socks of different colors.

### Examples

#### input

```
3 2 3
1 2 3
1 2
2 3
```

#### output

```
2
```

#### input

```
3 2 2
1 1 2
1 2
2 1
```

#### output

```
0
```

#### Note

In the first sample, Arseniy can repaint the first and the third socks to the second color.

In the second sample, there is no need to change any colors. 

## Solution

### 思路

利用并查集将要穿的袜子分成不同的集合。看每一个集合里哪个颜色的袜子最多，将其他的袜子染成同样的颜色即可。

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
#define MAXN 200010
#define INF 0x3f3f3f3f
#define DEBUG
#define DataIn
typedef long long LL;

using namespace std;

int par[MAXN], urank[MAXN];
int a[MAXN];
vector<int> b[MAXN];
int n, m, k;

void init()
{
    for(int i=1; i<=n; i++)
    {
        par[i] = i;
        urank[i] = 0;
    }
}

int ufind(int x)
{
    if(par[x] == x)
        return x;
    else
        return par[x] = ufind(par[x]);
}

void unite(int x, int y)
{
    x = ufind(x);
    y = ufind(y);
    if(x == y)
        return;
    if(urank[x] < urank[y])
        par[x] = y;
    else
    {
        par[y] = x;
        urank[x]++;
    }
}

bool same(int x, int y)
{
    return ufind(x) == ufind(y);
}

int main()
{
    scanf("%d%d%d", &n, &m, &k);
    init();
    for(int i=1; i<=n; i++)
        scanf("%d", a+i);

    int x, y;
    for(int i=1; i<=m; i++)
    {
        scanf("%d%d", &x, &y);
        unite(x, y);
    }

    for(int i=1; i<=n; i++)
        b[ufind(i)].push_back(a[i]);

    int ans = 0;
    for(int i=1; i<=n; i++)
    {
        if(b[i].size() <= 1)
            continue;
        map<int, int> mp;
        int maxx = 0;
        for(int j=0; j<b[i].size(); j++)
        {
            mp[b[i][j]]++;
            maxx = max(maxx, mp[b[i][j]]);
        }
        ans += b[i].size()-maxx;
    }
    printf("%d\n", ans);
    return 0;
}
```

![mark](http://cmhblog.cfzhao.com/blog/180803/987ggd9iHm.png)

| #                                                            | When                | Who                                                      | Problem                                                  | Lang    | Verdict                | Time   | Memory   |
| ------------------------------------------------------------ | ------------------- | -------------------------------------------------------- | -------------------------------------------------------- | ------- | ---------------------- | ------ | -------- |
| [41139275](http://codeforces.com/contest/731/submission/41139275) | 2018-08-03 04:20:15 | [MoonChasing](http://codeforces.com/profile/MoonChasing) | [C - Socks](http://codeforces.com/contest/731/problem/C) | GNU C++ | Accepted               | 202 ms | 9700 KB  |
| [41139257](http://codeforces.com/contest/731/submission/41139257) | 2018-08-03 04:18:37 | [MoonChasing](http://codeforces.com/profile/MoonChasing) | [C - Socks](http://codeforces.com/contest/731/problem/C) | GNU C++ | Accepted               | 187 ms | 11500 KB |
| [41139197](http://codeforces.com/contest/731/submission/41139197) | 2018-08-03 04:14:49 | [MoonChasing](http://codeforces.com/profile/MoonChasing) | [C - Socks](http://codeforces.com/contest/731/problem/C) | GNU C++ | Wrong answer on test 6 | 124 ms | 6600 KB  |