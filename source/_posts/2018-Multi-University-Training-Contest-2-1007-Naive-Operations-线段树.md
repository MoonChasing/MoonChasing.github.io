---
title: 2018 Multi-University Training Contest 2 1007 Naive Operations(线段树)
date: 2018-07-29 22:26:07
tags: [算法, 线段树]
categories: [ACM, 数据结构, 线段树]
---

传送门： [2018 Multi-University Training Contest 2 1007 Naive Operations](http://acm.hdu.edu.cn/showproblem.php?pid=6315)

## 题目

<h1 style="color:#1A5CC8;text-align:center">Naive Operations</h1>

**Time Limit: 6000/3000 MS (Java/Others)    Memory Limit: 502768/502768 K (Java/Others)** 

### Problem Description

In a galaxy far, far away, there are two integer sequence a and b of length n.<br>b is a static permutation of 1 to n. Initially a is filled with zeroes.<br>There are two kind of operations:<br>1. add l r: add one for $a_l,a_{l+1}...a_r$<br>2. query l r: query $\sum_{i=l}^r \lfloor a_i / b_i \rfloor$ <!--more-->

### Input

There are multiple test cases, please read till the end of input file.<br>For each test case, in the first line, two integers n,q, representing the length of a,b and the number of queries.<br>In the second line, n integers separated by spaces, representing permutation b.<br>In the following q lines, each line is either in the form 'add l r' or 'query l r', representing an operation.<br>$1 \leq n,q \leq 100000$, $1 \leq l \leq r \leq n$, there're no more than 5 test cases. 

### Output

Output the answer for each 'query', each one line. 

### Sample Input

> 5 12
>
> 1 5 2 4 3
>
> add 1 4
>
> query 1 4
>
> add 2 5
>
> query 2 5
>
> add 3 5
>
> query 1 5
>
> add 2 4
>
> query 1 4
>
> add 2 5
>
> query 2 5
>
> add 2 2
>
> query 1 5

### Sample Output

>1
>
>1
>
>2
>
>4
>
>4
>
>6

## 思路

看到题目就知道这个题目肯定是线段树。但是因为其非叶子结点的更新并不能在O(1)时间内得出，所以我们要对线段树进行特殊处理。

QAQ，关于想明白非叶子结点更新要在O(1)结点想出也是源于上海大都会赛上一个线段树的反思。等我明白后补上那题。

由$\lfloor a_i / b_i \rfloor$可知，因为是向下取整，所以只有每加$b_i$次后，叶子结点的值才会加1。于是我们要维护一个最小值，再维护一个叶子结点为0个数的和。每次更新，就把叶子结点-1，当叶子结点变成0后，再将其赋为$b_i$，而0的个数+1。

最终0个数的结果即为我们要求的值。

## 代码

```c++
#include <bits/stdc++.h>

using namespace std;

typedef long long LL;

const int MAXN = 1e5+5;
int n, q, a[MAXN], b[MAXN];

struct node
{
    LL minx, valb, sum_zero, lazy;
} SegTree[MAXN << 2];

void PushDown(int root, int l, int r)
{
    if(SegTree[root].lazy)
    {
        SegTree[root<<1].lazy += SegTree[root].lazy;
        SegTree[root<<1|1].lazy += SegTree[root].lazy;

        SegTree[root<<1].minx -= SegTree[root].lazy;
        SegTree[root<<1|1].minx -= SegTree[root].lazy;

        SegTree[root].lazy = 0;
    }
}

void PushUp(int root)
{
    SegTree[root].sum_zero = SegTree[root<<1].sum_zero + SegTree[root<<1|1].sum_zero;
    SegTree[root].minx = min(SegTree[root<<1].minx, SegTree[root<<1|1].minx);
}

void build(int root, int l, int r)
{
    SegTree[root].lazy = SegTree[root].sum_zero = 0;
    if(l == r)
    {
        SegTree[root].minx = SegTree[root].valb = b[l];
        SegTree[root].sum_zero = 0;
        return;
    }

    int mid = l+r>>1;
    build(root<<1, l, mid);
    build(root<<1|1, mid+1, r);
    PushUp(root);
}

void update(int root, int l, int r, int a, int b)
{
    if(l > r)
        return;

    if(SegTree[root].minx > 1 && a==l && b==r)
    {
        SegTree[root].lazy++;
        SegTree[root].minx--;
        return;
    }

    if(l == r && SegTree[root].minx == 1)
    {
        SegTree[root].sum_zero++;
        SegTree[root].lazy = 0;
        SegTree[root].minx = SegTree[root].valb;
        return;
    }

    PushDown(root, l, r);
    int mid = l+r>>1;

    if(b <= mid)
        update(root<<1, l, mid, a, b);
    else if(a > mid)
        update(root<<1|1, mid+1, r, a, b);
    else
    {
        update(root<<1, l, mid, a, mid);
        update(root<<1|1, mid+1, r, mid+1, b);
    }
    PushUp(root);
}

LL query(int root, int l, int r, int a, int b)
{
    if(l > r)
        return 0;
    if(l == a && r == b)
        return SegTree[root].sum_zero;
    if(SegTree[root].minx <= 0)
        update(1, 1, n, a, b);

    int mid = l+r>>1;
    if(b <= mid)
        return query(root<<1, l, mid, a, b);
    else if(a > mid)
        return query(root<<1|1, mid+1, r, a, b);
    else
        return query(root<<1, l, mid, a, mid) + query(root<<1|1, mid+1, r, mid+1, b);
}
int main()
{
     while (~scanf("%d %d", &n, &q)) {
        for (int i = 1; i <= n; i++)
            scanf("%d", &b[i]);
        build(1, 1, n);
        while (q--) {
            char op[10];
            int l, r;
            scanf("%s %d %d", op, &l, &r);
            if (op[0] == 'a') {
                update(1, 1, n, l, r);
            } else {
                LL ans = query(1, 1, n, l, r);
                printf("%lld\n", ans);
            }
        }
    }
    return 0;
}
```

## 总结

在这题AC代码中，自己学习了线段树一种新的更新方法。主要为这一步

```c++
int mid = l+r>>1;

if(b <= mid)
    update(root<<1, l, mid, a, b); //更新左边
else if(a > mid)
    update(root<<1|1, mid+1, r, a, b); //更新右边
else //两边同时更新
{
    update(root<<1, l, mid, a, mid);
    update(root<<1|1, mid+1, r, mid+1, b);
}
```

而原来，用的是

```c++
int mid = l+r>>1;
update(root<<1, l, mid, a, b, modify);
update(root<<1|1, mid+1, r, a, b, modify);
```

此题所用的代码中，通过对查询区间左右边界值的修改，可以达到`a==l && b==r`情况下的更新。