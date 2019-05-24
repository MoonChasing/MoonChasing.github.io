---
title: BZOJ 5488 Teamwork
date: 2019-05-19 23:29:59
tags: [dp, RMQ]
catagories: ACM
---

题目链接：[BZOJ 5488 Teamwork](https://www.lydsy.com/JudgeOnline/problem.php?id=5488)

# Problem

## Description

在Farmer John最喜欢的节日里，他想要给他的朋友们赠送一些礼物。由于他并不擅长包装礼物，他想要获得他的奶牛们的帮助。你可能能够想到，奶牛们本身也不是很擅长包装礼物，而Farmer John即将得到这一教训。Farmer John的N头奶牛（1≤N≤104）排成一行，方便起见依次编号为1…N。奶牛i的包装礼物的技能水平为si。她们的技能水平可能参差不齐，所以FJ决定把她的奶牛们分成小组。每一组可以包含任意不超过K头的连续的奶牛（1≤K≤103），并且一头奶牛不能属于多于一个小组。由于奶牛们会互相学习，这一组中每一头奶牛的技能水平会变成这一组中水平最高的奶牛的技能水平。请帮助FJ求出，在他合理地安排分组的情况下，可以达到的技能水平之和的最大值。<!--more-->

## Input

输入的第一行包含N和K。

以下N行按照N头奶牛的排列顺序依次给出她们的技能水平。技能水平是一个不超过10^5的正整数。

## Output

输出FJ通过将连续的奶牛进行分组可以达到的最大技能水平和。

## Sample Input

7 3
 1
 15
 7
 9
 2
 5
 10

## Sample Output

84

在这个例子中，最优的方案是将前三头奶牛和后三头奶牛分别分为一组，中间的奶牛单独成为一组（注意一组的奶牛数量可以小于K）。这样能够有效地将7头奶牛的技能水平提高至15、15、15、9、10、10、10，和为84。 

# Solution

## Idea

用 RMQ 查询区间内最大值， dp方程$ans[i] = max(ans[i], ans[j] + rmq(j+1, i) * (i-j));$

## Code

```C++
int n, k;

int a[MAXN];
int dp[MAXN][1<<4];
int mm[MAXN];
int ans[MAXN];

int rmq(int l, int r)
{
    int lg = mm[r-l+1];
    //printf("lg == %d\n", lg);
    return max(dp[l][lg], dp[r-(1<<lg)+1][lg]);
}
int main()
{
    //两种方式计算 2^x <= i
    //mm[0] = -1;
    for(int i=2; i<=MAXN; i++)
        mm[i] = mm[i>>1] + 1;
        //mm[i] = (0 == (i & (i-1) ) ) ? mm[i-1] + 1 : mm[i-1];

    scanf("%d%d", &n, &k);

    for(int i=1; i<=n; i++)
    {
        scanf("%d", a+i);
        dp[i][0] = a[i];
    }

    for(int j=1; j<=mm[n]; j++)
        for(int i=1; i+(1<<j)-1<=n; i++)
            dp[i][j] = max(dp[i][j-1], dp[i+(1<<(j-1))][j-1]);

    for(int i=1; i<=n; i++)
        for(int j=max(0, i-k); j<i; j++)
            ans[i] = max(ans[i], ans[j] + rmq(j+1, i) * (i-j));

    printf("%d\n", ans[n]);
    return 0;
}
```

