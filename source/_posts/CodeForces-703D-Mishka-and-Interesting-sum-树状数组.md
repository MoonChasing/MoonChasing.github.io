---
title: CodeForces 703D Mishka and Interesting sum(树状数组)
date: 2018-08-03 09:37:11
tags:
categories: [ACM, 数据结构, 树状数组]
---

<h2 align="center">D. Mishka and Interesting sum</h2>

<div align="center">time limit per test: 3.5 seconds<br>memory limit per test: 256 megabytes <br>input: standard input<br>output: standard output</div>

## Description

Little Mishka enjoys programming. Since her birthday has just passed, her friends decided to present her with array of non-negative integers *a*1, *a*2, ..., *an* of *n* elements!<!--more-->

Mishka loved the array and she instantly decided to determine its beauty value, but she is too little and can't process large arrays. Right because of that she invited you to visit her and asked you to process *m* queries.

Each query is processed in the following way:

1. Two integers *l* and *r* (1 ≤ *l* ≤ *r* ≤ *n*) are specified — bounds of query segment.
2. Integers, presented in array segment [*l*,  *r*] (in sequence of integers *a**l*, *a**l* + 1, ..., *a**r*) even number of times, are written down.
3. XOR-sum of written down integers is calculated, and this value is the answer for a query. Formally, if integers written down in point 2 are *x*1, *x*2, ..., *x**k*, then Mishka wants to know the value ![img](http://codeforces.com/predownloaded/ea/45/ea4547ffed8a3c8eb751acf0531e94388122e790.png), where ![img](http://codeforces.com/predownloaded/7b/ea/7beade55e90846d70020a3d03521d3458b66751b.png) — operator of exclusive bitwise OR.

Since only the little bears know the definition of array beauty, all you are to do is to answer each of queries presented.

### Input

The first line of the input contains single integer *n* (1 ≤ *n* ≤ 1 000 000) — the number of elements in the array.

The second line of the input contains *n* integers *a*1, *a*2, ..., *a**n* (1 ≤ *a**i* ≤ 109) — array elements.

The third line of the input contains single integer *m* (1 ≤ *m* ≤ 1 000 000) — the number of queries.

Each of the next *m* lines describes corresponding query by a pair of integers *l* and *r* (1 ≤ *l* ≤ *r* ≤ *n*) — the bounds of query segment.

### Output

Print *m* non-negative integers — the answers for the queries in the order they appear in the input.

### Examples

#### input

```
3
3 7 8
1
1 3
```

#### output

```
0
```

#### input

```
7
1 2 1 3 3 2 3
5
4 7
4 5
1 3
1 7
1 5
```

#### output

```
0
3
1
3
2
```

### Note

In the second sample:

There is no integers in the segment of the first query, presented even number of times in the segment — the answer is 0.

In the second query there is only integer 3 is presented even number of times — the answer is 3.

In the third query only integer 1 is written down — the answer is 1.

In the fourth query all array elements are considered. Only 1 and 2 are presented there even number of times. The answer is ![img](http://codeforces.com/predownloaded/f3/f4/f3f48ca9259e359e0f860214756305ac8291888b.png).

In the fifth query 1 and 3 are written down. The answer is ![img](http://codeforces.com/predownloaded/a1/ba/a1ba125fbec1b545761705318974b0be213f2ef2.png).

## Solution

### 题意

给你一个数组，若干个询问。询问区间 [l, r] 上出个次数为偶数的数的异或和。

### 思路

一个数字，奇数次异或和为其本身，偶数次异或和为0。一个区间上所有数字异或和为该区间上出现奇数次的数字的异或和。而现在让我们求偶数次数字的异或和， 我们可以再求得区间上所有distinct数字(去重)的异或和，再用此异或和和区间所有数字异或和进行异或操作，即为所求。

对于区间所有数字的异或和，我们可以先预处理出一个前缀异或和 $sum$，区间$[l,r]$ 上所有数字的异或和即为 `sum[r] ^ sum[l-1]`

区间上distinct数字的异或和我们用树状数组来维护。用一个 map 来维护某个数上次出现的位置，离线处理答案，走到 $a[i]$ 时，如果 $map[ a[i] ]$ 不为0，（说明 $a[i] $ 之前出现过）， 那么从上次的位置开始把 $a[i]$ 异或掉，并在此位置开始异或上 $a[i]$，更新map。只把出现多次的值放在最后出现的位置**（这个思想很常见）**。 

询问按区间右端点排序，依此顺序回答。

## AC代码

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
#define INF 0x3f3f3f3f
#define DEBUG
#define DataIn
typedef long long LL;

using namespace std;
const int MAXN = 1e6+10;

int n, q;
int a[MAXN], sum[MAXN], tree[MAXN<<2]; //sum为原数组前缀和
map<int, int> mp;   //mp[i]表示上一次i出现的下标

struct node
{
    int l, r;
    int no;
    int ans;
}Query[MAXN];

bool cmp(node a, node b)
{
    return a.r==b.r ? a.l<b.l : a.r<b.r;
}

bool cmp2(node a, node b)
{
    return a.no < b.no;
}

int lowbit(int k)
{
    return k&(-k);
}

void update(int ind, int val)
{
    while(ind <= n)
    {
        tree[ind] ^= val;
        ind += lowbit(ind);
    }
}

int query(int k)
{
    int ans = 0;
    while(k)
    {
        ans ^= tree[k];
        k -= lowbit(k);
    }
    return ans;
}

int main()
{
    scanf("%d", &n);
    for(int i=0; i<n; i++)
        scanf("%d", a+i);

    scanf("%d", &q);
    for(int i=0; i<q; i++)
    {
        scanf("%d%d", &Query[i].l, &Query[i].r);
        Query[i].no = i;
    }

    sum[0] = a[0];
    for(int i=1; i<n; i++)
        sum[i] ^= sum[i-1];

    sort(Query, Query+q, cmp);
    int p = 0;
    for(int i=0; i<q; i++)
    {
        int l=Query[i].l, r=Query[i].r;
        while(p<=r)
        {
            if(mp[a[p]] == 0)
            {
                mp[a[p]] = p;
                update(p, a[p]);
            }
            else
            {
                update(mp[a[p]], a[p]);	 //从上次的位置开始把 a[i] 异或掉
                update(p, a[p]);		//在此位置开始异或上 a[i]
                mp[a[p]] = p;
            }
            p++;
        }
        Query[i].ans = sum[r] ^ sum[l-1] ^ query(r) ^ query(l-1);
    }

    sort(Query, Query+q, cmp2);
    for(int i=0; i<q; i++)
        printf("%d\n", Query[i].ans);
    return 0;
}
```

