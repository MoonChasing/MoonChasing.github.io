---
title: 2018 Multi-University Training Contest 1 记录
date: 2018-07-24 10:09:42
tags: 算法
categories: 
- [ACM, 综合比赛]
- [ACM, 思维]
- [ACM, 构造]
- [ACM, 数论]
---

## 1001 Maximum Multiple

### Problem Description

Given an integer n, Chiaki would like to find three positive integers x, y and z such that: $n=x+y+z$, $x\mid n$, $y \mid n$, $z \mid n$ and $xyz$ is maximum. 

### Input

There are multiple test cases. The first line of input contains an integer $T$ ($1 \le T \le 10^6$), indicating the number of test cases. For each test case:<br>The first line contains an integer $n$ ($1 \le n \le 10^{6}$). 

### Output

For each test case, output an integer denoting the maximum $xyz$. If there no such integers, output $-1$ instead.  <!--more-->

### Sample Input

> 3
>
> 1
>
> 2
>
> 3

### Sample Output

> -1
>
> -1
>
> 1

### 思路

对 $n$ 是否是3，4的倍数进行讨论。

如果是 3 的倍数， 则 $ x = y = z =  {n \over  3} $； 如果是 4 的倍数， 则 $ x = {n \over 2}, y = z = {n \over 4} $。

若 $n$ 为 3 和 4 的公倍数，则按 3 的倍数处理。

### AC代码（队友）

```c++
#include<bits/stdc++.h>
using namespace std;

#define LL long long

int n;

int main() {
    int _;
    scanf( "%d", &_ );
    while( _-- ) {
        scanf( "%d", &n );
        if( n % 3 == 0 ) {
            LL ans = 1;
            ans = ( ans * n/3 * n/3 * n/3 );
            printf( "%lld\n", ans );
        } else if( n%4 == 0 ) {
            LL ans = 1;
            ans = ( ans * n/2 * n/4 * n/4 );
            printf( "%lld\n", ans );

        } else {
            puts( "-1" );
        }
    }

    return 0;
}
```

<hr>

## 1011 Time Zone

### Problem Description

Chiaki often participates in international competitive programming contests. The time zone becomes a big problem. Given a time in Beijing time (UTC +8), Chiaki would like to know the time in another time zone s. 

### Input

There are multiple test cases. The first line of input contains an integer $T$ ($1 \le T \le 10^6$), indicating the number of test cases. For each test case:<br>The first line contains two integers $a$, $b$ ($0 \le a \le 23, 0 \le b \le 59$) and a string $s$ in the format of &quot;UTC+X'', &quot;UTC-X'', &quot;UTC+X.Y'', or &quot;UTC-X.Y'' ($0 \le X, X.Y \le 14, 0 \le Y \le 9$). 

### Output

For each test, output the time in the format of $hh:mm$ (24-hour clock). 

### Sample Input

> 3
>
> 11 11 UTC+8
>
> 11 12 UTC+9
>
> 11 23 UTC+0

### Sample Output

> 11:11
>
> 12:12
>
> 03:23

### 思路

水题，时区转换，模拟即可。

如果让自己写可能会以分钟计数，应该可以省去进位的麻烦。

### AC代码（队友）

```c++
#include<bits/stdc++.h>
using namespace std;

#define LL long long

int h, m;
double change;
char str[100];

int main() {
    int _;
    scanf( "%d", &_ );
    while( _-- ) {
        scanf( "%d%d", &h, &m );
        getchar();
        getchar();
        getchar();
        getchar();
        scanf( "%lf", &change );
        if( (int)change == change ) {
            h += change-8;
            if( h < 0 ) {
                h += 24;
            } else if( h >= 24 ) {
                h -= 24;
            }
        } else {
            int _h = (int)change;
            double _m = change-_h;
            //cout<<m<<endl;
            if( _m < 0 ) {
                _m = (int)( _m * 10 +- 0.5 ) / 10.0;
            } else {
                _m = (int)( _m * 10 + 0.5 ) / 10.0;
            }

//            cout<<_m<<endl;
            h += _h-8;
            m += (int)(_m*60);
//            cout<<m<<endl;
            if( m < 0 ) {
                m += 60;
                h -= 1;
            } else if( m >= 60 ) {
                m -= 60;
                h += 1;
            }
            if( h < 0 ) {
                h += 24;
            } else if( h >= 24 ) {
                h -= 24;
            }
        }
        printf( "%02d:%02d\n", h, m );
    }
}
```

<hr>

## 1003 Triangle Partition

**Time Limit: 2000/1000 MS (Java/Others)    Memory Limit: 132768/132768 K (Java/Others)**

**Special Judge** 

### Problem Description

Chiaki has 3n points p1,p2,…,p3n. It is guaranteed that no three points are collinear. Chiaki would like to construct n disjoint triangles where each vertex comes from the 3n points. 

### Input

There are multiple test cases. The first line of input contains an integer $T$, indicating the number of test cases. For each test case:<br>The first line contains an integer $n$ ($1 \le n \le 1000$) -- the number of triangle to construct.<br>Each of the next $3n$ lines contains two integers $x_i$ and $y_i$ ($-10^9 \le x_i, y_i \le 10^9$).<br>It is guaranteed that the sum of all $n$ does not exceed $10000$. 

### Output

For each test case, output $n$ lines contain three integers $a_i,b_i,c_i$ ($1 \le a_i,b_i,c_i \le 3n$) each denoting the indices of points the $i$-th triangle use. If there are multiple solutions, you can output any of them. 

### Sample Input

> 1
>
> 1
>
> 1 2
>
> 2 3
>
> 3 5

### Sample Output

> 1 2 3

### 思路



把顶点按 x, y 坐标排序，依次构成三角形。

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
#define MAXN 3005
#define INF 0x3f3f3f3f
#define DEBUG
#define DataIn
typedef long long LL;

using namespace std;

int n;
struct point
{
    int id, x, y;
    bool operator < (point p)
    {
        if(x < p.x)
            return true;
        else if(x == p.x)
        {
            if(y < p.y)
                return true;
            else
                return false;
        }
        else
            return false;
    }
}P[MAXN];

int main()
{
    int T;
    scanf("%d", &T);
    while(T--)
    {
        scanf("%d", &n);
        for(int i=0; i<3*n; i++)
        {
            scanf("%d%d", &P[i].x, &P[i].y);
            P[i].id = i+1;
        }

        sort(P, P+3*n);

        for(int i=0; i<3*n; i++)
        {
            printf("%d ", P[i].id);
            if(i+1%3==0)
                putchar(10);
        }
    }
    return 0;
}
```

<hr>

## 1004 Distinct Values

**Time Limit: 4000/2000 MS (Java/Others)    Memory Limit: 32768/32768 K (Java/Others)** 

### Problem Description

Chiaki has an array of $n$ positive integers. You are told some facts about the array: for every two elements $a_i$ and $a_j$ in the subarray $a_{l..r}$ ($l \le i  \lt j \le r$), $a_i \ne a_j$ holds.<br>Chiaki would like to find a lexicographically minimal array which meets the facts. 

### Input

There are multiple test cases. The first line of input contains an integer $T$, indicating the number of test cases. For each test case:<br><br>The first line contains two integers $n$ and $m$ ($1 \le n, m \le 10^5$) -- the length of the array and the number of facts. Each of the next $m$ lines contains two integers $l_i$ and $r_i$ ($1 \le l_i \le r_i \le n$).<br><br>It is guaranteed that neither the sum of all $n$ nor the sum of all $m$ exceeds $10^6$. 

### Output

For each test case, output $n$ integers denoting the lexicographically minimal array. Integers should be separated by a single space, and no extra spaces are allowed  at the end of lines. 

### Sample Input

>3
>
>2 1
>
>1 2
>
>4 2 
>
>1 2
>
>3 4 
>
>5 2
>
>1 3 
>
>2 4 

### Samplt output

> 1 2
>
> 1 2 1 2 
>
> 1 2 3 1 1 

### 思路

用一个优先队列维护，优先使用较小的数字。使用完后注意放回。

### AC代码（队友）

```c++
#include<bits/stdc++.h>
using namespace std;

#define LL long long


const int N = 1e5+7;
struct node {
    int l ,r;
} P[N];

int n, m;
int ans[N];

int cmp(node a,node b)
{
    if(a.l!=b.r)
        return a.l<b.l;
    else
        return a.r<b.r;
}

int l, r;

priority_queue<int, vector<int>, greater<int> > Q;

int main() {
    int _;
    scanf( "%d", &_ );
    for( int i = 1; i <= 1e5; ++i ) {
        Q.push( i );
    }
    while( _-- ) {
        scanf( "%d%d", &n, &m );
        for( int i = 0; i < m; ++i ) {
            scanf( "%d%d", &P[i].l, &P[i].r );
        }
        sort( P, P+m, cmp );
        l = r = 1;
        ans[1] = 1;
        Q.pop();

        for( int i = 1; i <= n; ++i ) {
            ans[i] = 0;
        }
        for( int i = 0; i < m; ++i ) {
            int _l = P[i].l;
            int _r = P[i].r;
            for( int j = l; j < _l; ++j ) {
                Q.push( ans[j] );
            }
            l = max( l, _l );
            for( int j = r+1; j <= _r; ++j ) {
                ans[j] = Q.top();
                Q.pop();
            }
            r = max( r, _r );
        }

        for( int i = l; i <= r; ++i ) {
            Q.push( ans[i] );
        }
        for( int i = 1; i <= n; ++i ) {
            if( ans[i] == 0 ) {
                ans[i] = 1;
            }
            if( i == 1 ) {
                printf( "%d", ans[i] );
            } else {
                printf( " %d", ans[i] );
            }
        }
        puts( "" );
    }
}
```





















