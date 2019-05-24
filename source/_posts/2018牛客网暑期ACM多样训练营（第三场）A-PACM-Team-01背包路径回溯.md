---
title: 2018牛客网暑期ACM多样训练营（第三场）A.PACM Team 01背包路径回溯
date: 2018-07-27 21:12:12
tags: [算法, dp, 01背包]
categories: [ACM, DP]
---

传送门：[https://www.nowcoder.com/acm/contest/141/A](https://www.nowcoder.com/acm/contest/141/A)

## 题目描述   

Eddy was a contestant participating in ACM ICPC contests. ACM is short for Algorithm, Coding, Math. Since in the ACM contest, the most important knowledge is about algorithm, followed by coding(implementation ability), then math. However, in the ACM ICPC World Finals 2018, Eddy failed to solve a physics equation, which pushed him away from a potential medal.<!--more-->
 Since then on, Eddy found that physics is actually the most important thing in the contest. Thus, he wants to form a team to guide the following contestants to conquer the PACM contests(PACM is short for Physics, Algorithm, Coding, Math). 
 There are N candidate groups each composed of pi physics experts, ai algorithm experts, ci coding experts, mi math experts. For each group, Eddy can either invite all of them or none of them. If i-th team is invited, they will bring gi knowledge points which is calculated by Eddy's magic formula. Eddy believes that the higher the total knowledge points is, the better a team could place in a contest. But, Eddy doesn't want too many experts in the same area in the invited groups. Thus, the number of invited physics experts should not exceed P, and A for algorithm experts, C for coding experts, M for math experts.
 Eddy is still busy in studying Physics. You come to help him to figure out which groups should be invited such that they doesn't exceed the constraint and will bring the most knowledge points in total.

## 输入描述:

```
The first line contains a positive integer N indicating the number of candidate groups.
Each of following N lines contains five space-separated integer pi, ai, ci, mi, gi indicating that i-th team consists of pi physics experts, ai algorithm experts, ci coding experts, mi math experts, and will bring gi knowledge points.
The last line contains four space-separated integer P, A, C, M indicating the maximum possible number of physics experts, algorithm experts, coding experts, and math experts, respectively.

 1 ≤ N ≤ 36
 0 ≤ pi,ai,ci,mi,gi ≤ 36
 0 ≤ P, A, C, M ≤ 36
```

## 输出描述:

```
The first line should contain a non-negative integer K indicating the number of invited groups.
The second line should contain K space-separated integer indicating the index of invited groups(groups are indexed from 0).

You can output index in any order as long as each index appears at most once. If there are multiple way to reach the most total knowledge points, you can output any one of them. If none of the groups will be invited, you could either output one line or output a blank line in the second line.
```

### 示例1 

## 输入

```
2
1 0 2 1 10
1 0 2 1 21
1 0 2 1
```

## 输出

```
1
1
```

### 示例2

## 输入

```
1
2 1 1 0 31
1 0 2 1
```

## 输出

```
0
```

## 解法

其实就是01背包问题，只是背包的容量从一维变成了四维，并要求输出最优解方案。

## AC代码(1)

```c++
#include <bits/stdc++.h>

using namespace std;

const int MAXN = 37;

int n, p[MAXN], a[MAXN], c[MAXN], m[MAXN], g[MAXN];
short dp[MAXN][MAXN][MAXN][MAXN][MAXN];
bool path[MAXN][MAXN][MAXN][MAXN][MAXN];
int P, A, C, M;

int main()
{
    scanf("%d", &n);
    for(int i=0; i<n; i++)
        scanf("%d%d%d%d%d", p+i, a+i, c+i, m+i, g+i);
    scanf("%d%d%d%d", &P, &A, &C, &M);

    for(int i=n-1; i>=0; i--)
    {
        for(int ip=0; ip<=P; ip++)
        {
            for(int ia=0; ia<=A; ia++)
            {
                for(int ic=0; ic<=C; ic++)
                {
                    for(int im=0; im<=M; im++)
                    {
                        if(ip<p[i] || ia<a[i] || ic<c[i] || im<m[i])
                            dp[i][ip][ia][ic][im] = dp[i+1][ip][ia][ic][im];
                        else
                        {
                            if( dp[i+1][ip][ia][ic][im] < dp[i+1][ip-p[i]][ia-a[i]][ic-c[i]][im-m[i]] + g[i] )
                            {
                                dp[i][ip][ia][ic][im] = dp[i+1][ip-p[i]][ia-a[i]][ic-c[i]][im-m[i]] + g[i];
                                path[i][ip][ia][ic][im] = true;
                            }
                            else
                                dp[i][ip][ia][ic][im] = dp[i+1][ip][ia][ic][im];
                        }
                    }
                }
            }
        }
    }


        vector<int> ans;
        for(int i=0, ip=P, ia=A, ic=C, im=M; i<n; i++)
        {
            if(path[i][ip][ia][ic][im])
            {
                ans.push_back(i);
                ip -= p[i];
                ia -= a[i];
                ic -= c[i];
                im -= m[i];
            }
        }

        int sz = ans.size();
        printf("%d\n", sz);
        for(int i=0; i<sz; i++)
            printf("%d%c", ans[i], " \n"[i+1==sz]);
        if(sz==0)
            putchar(10);
        return 0;
    }
```

## AC代码(2)

```c++
#include <bits/stdc++.h>

using namespace std;

const int MAXN = 37;

int n, p[MAXN], a[MAXN], c[MAXN], m[MAXN], g[MAXN];
short dp[MAXN][MAXN][MAXN][MAXN][MAXN];
bool path[MAXN][MAXN][MAXN][MAXN][MAXN];
int P, A, C, M;

int main()
{
    scanf("%d", &n);
    for(int i=0; i<n; i++)
        scanf("%d%d%d%d%d", p+i, a+i, c+i, m+i, g+i);
    scanf("%d%d%d%d", &P, &A, &C, &M);

    for(int i=n-1; i>=0; i--)
    {
        for(int ip=0; ip<=P; ip++)
        {
            for(int ia=0; ia<=A; ia++)
            {
                for(int ic=0; ic<=C; ic++)
                {
                    for(int im=0; im<=M; im++)
                    {
                        //if(ip<p[i] || ia<a[i] || ic<c[i] || im<m[i])
                            dp[i][ip][ia][ic][im] = dp[i+1][ip][ia][ic][im];
                    }
                }
            }
        }

        for(int ip=P; ip>=p[i]; ip--)
            for(int ia=A; ia>=a[i]; ia--)
                for(int ic=C; ic>=c[i]; ic--)
                    for(int im=M; im>=m[i]; im--)
                    {
                            if(dp[i+1][ip-p[i]][ia-a[i]][ic-c[i]][im-m[i]]+g[i] > dp[i+1][ip][ia][ic][im])
                            {
                                dp[i][ip][ia][ic][im] = dp[i+1][ip-p[i]][ia-a[i]][ic-c[i]][im-m[i]]+g[i];
                                path[i][ip][ia][ic][im] = true;
                            }
                    }
    }


    fprintf(stderr, "%d\n", dp[0][P][A][C][M]);
    vector<int> ans;


    for(int i=0, ip=P, ia=A, ic=C, im=M; i<n; i++)
    {
        //if(dp[i][ip][ia][ic][im] == dp[i+1][ip-p[i]][ia-a[i]][ic-c[i]][im-m[i]]+g[i])
        if(path[i][ip][ia][ic][im])
        {
            ans.push_back(i);
            ip -= p[i];
            ia -= a[i];
            ic -= c[i];
            im -= m[i];
        }
    }

    int sz = ans.size();
    printf("%d\n", sz);
    for(int i=0; i<sz; i++)
        printf("%d%c", ans[i], " \n"[i+1==sz]);
    if(sz==0)
        putchar(10);
    return 0;
}
```

## 总结

以上两个代码主要思想是一样的，只要在细节上的实现不一样。

这个题因为是四维的背包，再加上一维，空间耗费极大。而所维护的数值范围很小，所以使用 short 类型的数组。（骚呀，第一次遇到这样的，比赛时卡了空间）![mark](http://cmhblog.cfzhao.com/blog/180727/CCHc2381Bf.png)

因为要回溯路径，所以要用五维的数组，四维的数组保存不了路径，会被破坏。![mark](http://cmhblog.cfzhao.com/blog/180727/HK7D3JgCkm.png)

（不过 lyy 说通过压位可以）

自己用的白书上的模板

```c++
for(int i=n-1; i>=0; i++)
{
    for(int j=0; j<=W; j++)
    {
        if(j < w[i])
            dp[i][j] = dp[i+1][j];
        else
            dp[i][j] = max(dp[i+1][j], dp[i+1][j-w[i]]+v[i]);
    }
}
```

由于需要回溯路径， 所以 `dp[i][j] = max(dp[i+1][j], dp[i+1][j-w[i]]+v[i]);`一行不再适用，需要分开判断。而在比赛时自己就在这里犯了错。自己只写了

```c++
if(dp[i+1][j-w[i]]+v[i] > dp[i+1][j])
    dp[i][j] = dp[i+1][j-w[i]]+v[i];
```

而没写

```c++
else
    dp[i][j] = dp[i+1][j];
```

这个点卡了自己很久，用了几乎两个晚上找到！

还有一点就是路径的回溯， 要用一个 bool 类型的 path 数组来记录在此位置是否进行更改。自己之前是用的记录跳跃的位置，耗费了很多的空间。

这题可以说是到目前为止自己耗时最多的题目了，现在对于01背包清楚了许多！以后不怕了，记录下解题的曲折路程。

![mark](http://cmhblog.cfzhao.com/blog/180727/6b4181A4iI.png)![mark](http://cmhblog.cfzhao.com/blog/180727/8mhgA2j7aB.png)![mark](http://cmhblog.cfzhao.com/blog/180727/98EkC2jI58.png)

（中间一次过掉是用了标程）