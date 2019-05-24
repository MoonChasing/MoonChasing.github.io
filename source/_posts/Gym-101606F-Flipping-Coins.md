---
title: Gym-101606F Flipping Coins
date: 2018-05-08 11:11:30
tags: [dp, 算法]
categories: ACM
---

![题目](http://cmhblog.cfzhao.com/%E6%8D%95%E8%8E%B7.PNG)

* 题意：给你一些硬币，硬币的初始状态都是正面向下，反面向上，现在你可以每次任取一个硬币把它抛向空中，你可以进行k次操作，问你k次操作之后硬币正面向上的最大数学期望。<!--more-->

* 思路：一共进行k次操作，每次我们都可以任取一个硬币把它抛一次，由于题目中说是让求最大的数学期望，那么我们抛的时候取正面向下的可以得到最大的数学期望，我们定义一个状态dp[i][j]，表示抛i次其中有j个硬币正面向上的概率。

  那么我们可以得到转移方程：

  **dp\[i+1][j]+=dp\[i][j]*0.5;**

  **dp\[i+1][j+1]+=dp\[i][j]*0.5;**

  当j=n时

  **dp\[i+1][n]+=dp\[i][n]*0.5;**

  **dp\[i+1][n-1]+=dp\[i][n]*0.5;**

  **注意dp\[0][0]=1;**

* AC代码：

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
#define MAXN 410
#define INF 0x3f3f3f3f
#define DEBUG
#define DataIn
typedef long long LL;

using namespace std;

double dp[MAXN][MAXN];
int main()
{
    int n,k;
    scanf("%d%d", &n, &k);
    memset(dp, 0, sizeof(dp));

    dp[0][0] = 1;

    for(int i=0; i<k; i++)
    {
        for(int j=0; j<n; j++)
        {
            dp[i+1][j] += dp[i][j]*0.5;
            dp[i+1][j+1] += dp[i][j]*0.5;
        }
        dp[i+1][n] += dp[i][n]*0.5;
        dp[i+1][n-1] += dp[i][n]*0.5;
    }

    double ans = 0;
    for(int i=1; i<=n; i++)
    {
        ans += dp[k][i] * i;
    }
    printf("%f\n",ans);
}
```

