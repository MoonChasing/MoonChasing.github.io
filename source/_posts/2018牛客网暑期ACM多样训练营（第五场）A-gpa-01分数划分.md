---
title: 2018牛客网暑期ACM多校训练营（第五场）A gpa 01分数规划
date: 2018-08-03 19:51:28
tags: 01分数规划
categories: [ACM, 01分数规划]
---

传送门： [2018牛客网暑期ACM多样训练营（第五场）A gpa](https://www.nowcoder.com/acm/contest/143/A)

## Description

### 题目描述   

  At the university where she attended, the final score of her is  ${\sum{s[i]c[i]} \over \sum{s[i]}}$ 

  Now she can delete at most k courses and she want to know what the highest final score that can get.  <!--more-->

### 输入描述:

```
The first line has two positive integers n,k

The second line has n positive integers s[i]

The third line has n positive integers c[i]
```

### 输出描述:

```
Output the highest final score, your answer is correct if and only if the absolute error with the standard answer is no more than 10-5
```

### 示例1 

#### 输入

```
3 1
1 2 3
3 2 1
```

#### 输出

```
2.33333333333
```

### 说明

```
Delete the third course and the final score is 
```

### 备注

```
1≤ n≤ 105

0≤ k < n

1≤ s[i],c[i] ≤ 103
```
## Solution

### 题意

大学里有n门课，每门课有着自己的学分，gpa的计算公式为 ${\sum{分数 * 学分} \over {\sum学分}}$， 现可以最多除去$k$门课成绩，问绩点最多可以达到多少。

### 思路

这题一看就不可以贪心。是一个01分数规划的问题(模板题)，自己之前在白书上看过，做到这题时忘了，多谢[朱兄](https://blog.csdn.net/Pandauncle)的提醒。

### 讲解

![mark](http://cmhblog.cfzhao.com/blog/180805/abd5763gC5.png)![mark](http://cmhblog.cfzhao.com/blog/180805/K0haJIk0jj.png)![mark](http://cmhblog.cfzhao.com/blog/180805/ldK1adHFdA.png)

### AC代码

```c++
#include <iostream>
#include <string>
#include <cstdio>
#include <cstring>
#include <queue>
#include <algorithm>
using namespace std;
const int maxn=1e5+7;
const double eps=1e-7;
int n,k;
double a[maxn];
double b[maxn];
int main()
{
    while(cin>>n>>k)
    {
        if(n==0&&k==0)break;
      for(int i=0;i<n;i++)
       scanf("%lf",&b[i]);
      for(int j=0;j<n;j++)
       scanf("%lf",&a[j]);
       for( int j = 0; j < n; ++j ) {
        a[j] = a[j] * b[j];
       }
 
      double L=1.0;
      double R=1000.0;
      double mid;
 
      double t[maxn];
 
      while(R-L>eps)
      {
         mid=(R+L)*1.0/2;
 
         for(int i = 0; i < n; i++)
          t[i] = a[i] - mid * b[i];
         sort(t, t + n);
         double sum = 0;
         for(int i = k; i < n; i++)
          sum += t[i];
 
        if(sum>0)
            L=mid;
        else
            R=mid;
      }
      printf("%.5f\n",mid);
      break;
    }
    return 0;
}
```

