---
title: LCS算法
date: 2018-05-08 10:33:27
tags: [算法, 字符串]
categories: ACM
---

<!--more-->
```c++
#include <bits/stdc++.h>
#define MAXN 105
using namespace std;

int main()
{
    char str1[MAXN], str2[MAXN];
    int dp[MAXN][MAXN];
    int path[MAXN][MAXN];

    puts("Please input the first string:");
    gets(str1);
    puts("Please input the second string:");
    gets(str2);
    int len1 = strlen(str1), len2 = strlen(str2);

    memset(dp, 0, sizeof(dp));
    for(int i=0; i<len1; i++)
        for(int j = 0; j<len2; j++)
        {
            if(str1[i] == str2[j])
            {
                dp[i+1][j+1] = dp[i][j] + 1;
                path[i+1][j+1] = 1;
            }
            else
            {
                dp[i+1][j+1] = max(dp[i+1][j], dp[i][j+1]);
                path[i+1][j+1] = dp[i+1][j] > dp[i][j+1] ? 2 : 3;
            }
        }

    int ans = dp[len1][len2];
    printf("The length of LCS is %d\n", ans);
    int cnt = ans-1;
    char LCS[MAXN];
    int i=len1, j=len2;
    while(i>0 && j>0)
    {
        if(path[i][j] == 1)
        {
            LCS[cnt--] = str1[i-1];
            i--, j--;
        }
        else if(path[i][j] == 2)
            j--;
        else
            i--;
    }
    puts(LCS);
}
```

&emsp;&emsp;可以说，这是自己第一次写程序来保存路径，之前自己总觉得在数组中保存路径是保存确定的位置，经过编码自己了解，其实是要保存转移的方向，再根据方向来回溯。

&emsp;&emsp;编程真的要细心呀，这个程序自己在程序课上敲码5分钟，Debug 两节课。原因竟是`else if(path[i][j] == 2)`中的==写错，少打了一个等号，弱智错误呀，调试了两节课才找到。不过找到的时候蛮玄学的，是自己用常用的`#ifdef DEBUG`大法时，再判等时想起应该是两个等号， 忽地一想，自己不会是之前判等写错了吧，眼光向下一移，果然....解决。事迹已加入脑残系列，可以与上次离散实验中的`if(ischuandi());`相并列了。