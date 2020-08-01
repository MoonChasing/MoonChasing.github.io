---
title: CodeForces 825F String Compression(KMP求循环节+DP)
date: 2018-08-03 13:40:34
tags: [dp, 字符串]
categories: 
  - [ACM, DP]
  - [ACM, 字符串, KMP]
---

<h2 align="center">F. String Compression  </h2>

<div align="center">time limit per test: 2 seconds<br>memory limit per test: 512 megabytes<br>input: standard input<br>output: standard output</div>

## Description

Ivan wants to write a letter to his friend. The letter is a string *s* consisting of lowercase Latin letters.

Unfortunately, when Ivan started writing the letter, he realised that it is very long and writing the whole letter may take extremely long time. So he wants to write the compressed version of string *s* instead of the string itself.<!--more-->

The compressed version of string *s* is a sequence of strings *c*1, *s*1, *c*2, *s*2, ..., *c**k*, *s**k*, where *c**i* is the decimal representation of number *a**i*(without any leading zeroes) and *s**i* is some string consisting of lowercase Latin letters. If Ivan writes string *s*1 exactly *a*1 times, then string *s*2 exactly *a*2 times, and so on, the result will be string *s*.

The length of a compressed version is |*c*1| + |*s*1| + |*c*2| + |*s*2|... |*c**k*| + |*s**k*|. Among all compressed versions Ivan wants to choose a version such that its length is minimum possible. Help Ivan to determine minimum possible length.

### Input

The only line of input contains one string *s* consisting of lowercase Latin letters (1 ≤ |*s*| ≤ 8000).

### Output

Output one integer number — the minimum possible length of a compressed version of *s*.

### Examples

#### input

```
aaaaaaaaaa
```

#### output

```
3
```

#### input

```
abcab
```

#### output

```
6
```

#### input

```
cczabababab
```

#### output

```
7
```

## Solution

### 思路

KMP求循环节：如果 $len \% (len-next[len] == 0)$, 则字符串由循环节组成，循环节长度为 $len - next[len]$，循环 $len \over {len-next[len]}$次；否则字符串并无循环节。

$a[i][j]$表示子字符串$s[i,i+1, ....j]$循环节长度 + 循环次数的位数。

$dp[i]$表示字符串前 $i$ 个字符压缩后的长度。

$dp[i] = min(dp[i], dp[j]+a[j+1][i])$

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
#define MAXN 8005
#define INF 0x3f3f3f3f
#define DEBUG
#define DataIn
typedef long long LL;

using namespace std;
int next[MAXN];
char s[MAXN];
int len;
int dp[MAXN];
int a[MAXN][MAXN];

void prekmp(char x[])
{
    int i = 0, j = -1;
    next[0] = -1;
    while(i < len)
    {
        while(j!=-1 && x[i]!=x[j])
            j = next[j];
        next[++i] = ++j;
    }
}

int digitcnt(int k)
{
    int cnt = 0;
    while(k)
    {
        k /= 10;
        cnt++;
    }
    return cnt;
}

void init()
{
    for(int i = 0; i < len; i++)
    {
        prekmp(s + i);
        for(int j = i; j < len; j++)
        {
            if(j == i)
            {
                a[i][j] = 2;
                continue;
            }

            int t1 = j - i + 1, t2 = t1 - next[t1];
            a[i][j] = t1 % t2 == 0 ? t2 + digitcnt(t1/t2) : t1 + 1;
        }
    }
}

int main()
{
    scanf("%s", s);
    len = strlen(s);
    fill(dp, dp+len, INF);
    init();
    dp[0] = 2;
    for(int i = 1; i < len;i ++)
    {
        dp[i] = a[0][i];
        for(int j = 0; j < i; j ++)
            dp[i] = min(dp[i],dp[j] + a[j + 1][i]);
    }
    printf("%d\n",dp[len-1]);

    return 0;
}
```

![mark](http://cmhblog.cfzhao.com/blog/180803/DHJIgDCe8b.png)

| #                                                            | When                | Who                                                      | Problem                                                      | Lang    | Verdict  | Time   | Memory    |
| ------------------------------------------------------------ | ------------------- | -------------------------------------------------------- | ------------------------------------------------------------ | ------- | -------- | ------ | --------- |
| [41132032](http://codeforces.com/contest/825/submission/41132032) | 2018-08-02 21:00:29 | [MoonChasing](http://codeforces.com/profile/MoonChasing) | [F - String Compression](http://codeforces.com/contest/825/problem/F) | GNU C++ | Accepted | 810 ms | 250900 KB |