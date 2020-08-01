---
title: Google KickStart 2019B Circuit Board
date: 2019-05-29 18:25:42
tags: 思维
---

 

传送门：[Circuit Board](https://codingcompetitions.withgoogle.com/kickstart/round/0000000000050ff2/0000000000150aae)

## Problem

### Description

Arsh recently found an old rectangular circuit board that he would like to recycle. The circuit board has **R** rows and **C** columns of squares.

Each square of the circuit board has a thickness, measured in millimetres. The square in the r-th row and c-th column has thickness **Vr,c**. A circuit board is *good* if in each row, the difference between the thickest square and the least thick square is no greater than **K**.

Since the original circuit board might not be good, Arsh would like to find a good subcircuit board. A subcircuit board can be obtained by choosing an axis-aligned subrectangle from the original board and taking the squares in that subrectangle. Arsh would like your help in finding the number of squares in the largest good subrectangle of his original board.<!--more-->

### Input

The first line of the input gives the number of test cases, **T**. **T** test cases follow. Each test case begins with one line containing three integers **R**, **C** and **K**, the number of rows, the number of columns, and the maximum difference in thickness allowed in each row.

Then, there are **R** more lines containing **C** integers each. The c-th integer on the r-th line is **Vr, c**, the thickness of the square in the r-th row and c-th column.

### Output

For each test case, output one line containing `Case #x: y`, where `x` is the test case number (starting from 1) and `y` is the maximum number of squares in a good subrectangle.

### Limits

Time limit: 15 seconds per test set.
Memory limit: 1GB.
1 ≤ **T** ≤ 50.
1 ≤ **R** ≤ 300.
1 ≤ **C** ≤ 300.
0 ≤ **Vi, j** ≤ 103 for all i, j.

#### Test set 1 (Visible)

**K** = 0.

#### Test set 2 (Hidden)

0 ≤ **K** ≤ 103.

### Sample

| Input 1                                                      | Output 1                                |
| ------------------------------------------------------------ | --------------------------------------- |
| `3 1 4 0 3 1 3 3 2 3 0 4 4 5 7 6 6 4 5 0 2 2 4 4 20 8 3 3 3 12 6 6 3 3 3 1 6 8 6 4   ` | `Case #1: 2 Case #2: 2 Case #3: 6     ` |

| Input 2                                                      | Output 2                               |
| ------------------------------------------------------------ | -------------------------------------- |
| `3 1 4 2 3 1 3 3 3 3 2 0 5 0 8 12 3 7 10 1 4 4 8 20 10 20 10 10 4 5 20 20 5 4 10 10 20 10 20   ` | `Case #1: 4 Case #2: 3 Case #3: 4    ` |

Note: Sample 2 is not valid for Test set 1. Sample 1 will be tested prior to running Test set 1 (the same way samples normally are). However, Sample 2 will **not** be tested prior to running Test set 2.

The sample cases are illustrated below. For each case, the good subcircuit board with the largest number of squares is highlighted in green.

![mark](http://cmhblog.cfzhao.com/blog/20190602/X3xP93s15bBe.JPG)

## Solution

### Code

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
#define MAXN 305
#define INF 0x3f3f3f3f
#define DEBUG
#define DataIn
typedef long long LL;

using namespace std;

int T, r, c, k;
int v[305][305];
int dp[305][305];


int main()
{
    scanf("%d", &T);

    for(int kiss = 1; kiss <= T; kiss++)
    {
        scanf("%d%d%d", &r, &c, &k);
        for(int i=1; i<=r; i++)
        {
            for(int j=1; j<=c; j++)
                scanf("%d", &v[i][j]);
        }

        for(int i=1; i<=r; i++)
        {
            dp[i][c] = 1;
            for(int j=c-1; j>=1; j--)
            {
                int minn = v[i][j];
                int maxx = v[i][j];
                dp[i][j] = c-j+1;

                for(int f = j+1; f<=c; f++)
                {
                    if(v[i][f] > maxx)
                        maxx = v[i][f];
                    if(v[i][f] < minn)
                        minn= v[i][f];
                    if(maxx - minn > k)
                    {
                        dp[i][j] = f-j;
                        break;
                    }
                }

            }
        }

        int maxx = r;

        for(int i=1; i<=r; i++)
        {
            for(int j=1; j<=c; j++)
            {
                priority_queue<int, vector<int>, greater<int> > que;
                for(int f=i; f<=r; f++)
                {
                    que.push(dp[f][j]);
                    maxx = max(maxx, que.top() * (f-i+1));
                }
            }
        }

        printf("Case #%d: %d\n", kiss, maxx);
    }
    return 0;
}
```

