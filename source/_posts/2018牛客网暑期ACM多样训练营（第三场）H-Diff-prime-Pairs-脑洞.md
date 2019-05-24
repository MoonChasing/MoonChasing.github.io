---
title: 2018牛客网暑期ACM多样训练营（第三场）H.Diff-prime Pairs 脑洞
date: 2018-07-27 22:29:58
tags:
categories: [ACM, 思维]
---

传送门：[https://www.nowcoder.com/acm/contest/141/H](https://www.nowcoder.com/acm/contest/141/H)

## 题目描述   

Eddy has solved lots of problem involving calculating the number of coprime pairs within some range. This problem can be solved with inclusion-exclusion method. Eddy has implemented it lots of times. Someday, when he encounters another coprime pairs problem, he comes up with diff-prime pairs problem.<!--more--> diff-prime pairs problem is that given N, you need to find the number of pairs (i, j), where $ i \over gcd(i,j)$  and  $j \over gcd(i, j)$ are both prime and i ,j ≤ N. gcd(i, j) is the greatest common divisor of i and j. Prime is an integer greater than 1 and has only 2 positive divisors.
 Eddy tried to solve it with inclusion-exclusion method but failed. Please help Eddy to solve this problem.
 Note that pair (i1, j1) and pair (i2, j2) are considered different if i1 ≠ i2 or j1 ≠ j2.

## 输入描述:

```
Input has only one line containing a positive integer N.

1 ≤ N ≤ 107
```

## 输出描述:

```
Output one line containing a non-negative integer indicating the number of diff-prime pairs (i,j) where i, j ≤ N
```

## 示例1

### 输入

```
3
```

### 输出

```
2
```

## 示例2

### 输入

```
5
```

### 输出

```
6
```
## 思路

首先用数组 primecnt 来记录从 1 到 n 之间素数的个数。

$\sum_{i=1}^nprimecnt[\lfloor {n \over i}\rfloor]*(primecnt[\lfloor {n \over i}\rfloor]-1)$ 即为答案

## AC代码

```c++
#include<bits/stdc++.h>
using namespace std;

const int MAXN = 1e7+1;

bool notprime[MAXN];
int primecnt[MAXN];
typedef long long LL;

void init()
{
    memset(notprime, false, sizeof(notprime));
    notprime[0] = notprime[1] = true;
    for(int i=2; i<MAXN; i++)
    {
        if(!notprime[i])
        {
            if( i > MAXN/i )
            {
                continue;
            }
            for(int j=i*i; j<MAXN; j+=i)
            {
                notprime[j] = true;
            }
        }
    }

    primecnt[0] =primecnt[1] = 0;
    for(int i=2; i<MAXN; i++)
    {
        if(!notprime[i])
            primecnt[i] = primecnt[i-1]+1;
        else
            primecnt[i] = primecnt[i-1];
    }
}



int sum = 0;
int n;
int temp;
LL ans = 0;
int main()
{
    init();
//    for(int i=1; i<100; i++)
//       printf("%d:%d\n", i, primecnt[i]);
    scanf("%d", &n);
    for(int i=1; i<=n; i++)
    {
        temp = primecnt[n/i];

        ans += (LL)(1LL*(temp-1)*temp);
    }
    printf("%lld\n", ans);
    return 0;
}
```

## 总结

注意数据类型转换！ `ans += (LL)(temp * (temp-1) );`是错误的！不能把 `temp * (temp-1)`转换为 long long。应该使用 (LL)temp * (temp-1)。以后再在这上面犯错剁手！

再用 %d 输出 long long 剁手！