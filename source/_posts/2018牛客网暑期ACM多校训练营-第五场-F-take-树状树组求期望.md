---
title: 2018牛客网暑期ACM多校训练营(第五场) F.take (树状树组求期望)
date: 2018-08-21 22:22:06
tags:
categories: [ACM, 数据结构, 树状数组]
---

传送门: [2018牛客网暑期ACM多校训练营(第五场) F.take](https://www.nowcoder.com/acm/contest/143/F)

## Problem

### Description

Kanade has n boxes , the i-th box has p[i] probability to have an diamond of d[i] size.  

At the beginning , Kanade has a diamond of 0 size. She will open the boxes from 1-st to n-th. When she open a box,if there is a diamond in it and it's bigger than the diamond of her , she will replace it with her diamond.  

Now you need to calculate the expect number of replacements.  

You only need to output the answer module 998244353.  ![take](https://cdn.pixabay.com/photo/2018/05/10/22/42/ice-cream-3389010_960_720.jpg)<!--more-->

Notice: If x%998244353=y*d %998244353 ,then we denote that x/y%998244353 =d%998244353  

### Input:

```
The first line has one integer n.
Then there are n lines. each line has two integers p[i]*100 and d[i].
```

### Output:

```
Output the answer module 998244353
```

### Sample input

```
3
50 1
50 2
50 3
```

### Sample output

```
499122178
```

### Hint:

```
1<= n <= 100000
1<=p[i]*100 <=100
1<=d[i]<=10^9
```

## Solution

### Idea

取当前钻石的概率为之前的比这个钻石大的皆不取的概率乘取这个的概率，即 $\prod_{j<i, size_j>size_i}(1-p_j) \times p_i$,我们可以对钻石先排个序，使（个大的） 或 （同大但先出现的）排在前边，用一个树状数组来维护 $1-p_i$的前缀积。

### AC代码

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long LL;
const int MOD = 998244353;
const int MAXN = 1<<17;
int n;
LL bit[MAXN];

LL qpow_mod(LL a, LL b = MOD-2)
{
    LL ret = 1;
    while(b)
    {
        if(b & 1)
            ret = ret * a % MOD;
        b >>= 1;
        a = a*a%MOD;
    }
    return ret;
}

struct node
{
    int id, sz, p;
    bool operator < (const node &a) const
    {
        return sz > a.sz || sz == a.sz && id < a.id;
    }
} P[MAXN];

void update(LL x, LL v)
{
    for(LL i=x; i <= n; i += i&-i )
        bit[i] = bit[i]*v%MOD;
}

LL query(LL x)
{
    LL ret = 1;
    while(x)
    {
        ret = (ret * bit[x]) % MOD;
        x -= x&-x;
    }
    return ret;
}


int main()
{
    cin >> n;
    LL ans = 0;
    LL inv0 = qpow_mod(100);
    for(int i=0; i<MAXN; i++)
        bit[i] = 1;
    for(int i=0; i<n; i++)
    {
        cin >> P[i].p >> P[i].sz;
        P[i].id = i + 1;
    }
    sort(P, P+n);
    LL temp;
    for(int i=0; i<n; i++)
    {
        temp = (query(P[i].id-1) * (inv0*P[i].p%MOD) )%MOD;
        ans = (ans+temp)%MOD;
        update(P[i].id, inv0*(100-P[i].p)%MOD);
    }
    cout << ans <<endl;
    return 0;
}
```

### Note

之前的自己以为不可能做出这种题目，好好补题后发现其实它并没有自己想得那么可怕，那些逆元什么的只要知道基础知识就好了。