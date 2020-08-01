---
title: POJ 1321 棋盘问题(DFS+回溯)
date: 2018-08-10 11:19:13
tags: kuangbin带你飞
categories: [ACM, DFS]
---

传送门：[棋盘问题](http://poj.org/problem?id=1321)

<blockquote class="blockquote-center">人一我百！人十我万！永不放弃~~~ 怀着自信的心，去追逐梦想 ——kuangbin </blockquote>

## Problem

### Description

在一个给定形状的棋盘（形状可能是不规则的）上面摆放棋子，棋子没有区别。要求摆放时任意的两个棋子不能放在棋盘中的同一行或者同一列，请编程求解对于给定形状和大小的棋盘，摆放k个棋子的所有可行的摆放方案C。<!--more-->

### Input

输入含有多组测试数据。 
每组数据的第一行是两个正整数，n k，用一个空格隔开，表示了将在一个n*n的矩阵内描述棋盘，以及摆放棋子的数目。 n <= 8 , k <= n 
当为-1 -1时表示输入结束。 
随后的n行描述了棋盘的形状：每行有n个字符，其中 # 表示棋盘区域， . 表示空白区域（数据保证不出现多余的空白行或者空白列）。 

### Output

对于每一组数据，给出一行输出，输出摆放的方案数目C （数据保证$C \le {2^{31}} $）。

### Sample Input

```
2 1
#.
.#
4 4
...#
..#.
.#..
#...
-1 -1
```

### Sample Output

```
2
1
```

## Solution

### AC代码

```c++
int ans;
int n, k;
char mz[10][10];
bool visrow[10], viscol[10];

void dfs(int i, int used)
{
    if(used == k)
    {
        ans++;
        return;
    }
    if(i > n)
        return;


    for(int j=1; j<=n; j++)
    {
        if(mz[i][j] == '#')
        {
            if(!visrow[i] && !viscol[j])
            {
                visrow[i] = true;
                viscol[j] = true;

                dfs(i+1, used+1);

                visrow[i] = false;
                viscol[j] = false;
            }
        }
    }
    dfs(i+1, used);

}

int main()
{
    while(scanf("%d%d", &n, &k) && (~n || ~k))
    {
        ans = 0;
        memset(visrow, false, sizeof(visrow));
        memset(viscol, false, sizeof(viscol));
        memset(mz, '.', sizeof(mz));
        getchar();
        for(int i=1; i<=n; i++)
        {
            for(int j=1; j<=n; j++)
                mz[i][j] = getchar();
            getchar();
        }
        dfs(1, 0);
        printf("%d\n", ans);
    }
    return 0;
}
```

## Note

还是觉得自己太菜了，于是决定刷一遍kuangbin带你飞。决心要把每一道题弄会。

上传代码会精简一些，不放头文件堆叠等一些代码。

<p align="right">2018年8月10日</p>

<p align="right">于学校</p>