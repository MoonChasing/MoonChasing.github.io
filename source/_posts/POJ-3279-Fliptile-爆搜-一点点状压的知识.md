---
title: POJ 3279 Fliptile(爆搜+一点点状压的知识)
date: 2018-08-13 17:47:05
tags: kuangbin带你飞
categories: [ACM]
---

传送门：[Fliptile](http://poj.org/problem?id=3279)

<blockquote class="blockquote-center">人一我百！人十我万！永不放弃~~~ 怀着自信的心，去追逐梦想 ——kuangbin </blockquote>

## Problem

### Description

Farmer John knows that an intellectually satisfied cow is a happy cow who will give more milk. He has arranged a brainy activity for cows in which they manipulate an *M* × *N* grid (1 ≤ *M* ≤ 15; 1 ≤ *N* ≤ 15) of square tiles, each of which is colored black on one side and white on the other side.<!--more-->

As one would guess, when a single white tile is flipped, it changes to black; when a single black tile is flipped, it changes to white. The cows are rewarded when they flip the tiles so that each tile has the white side face up. However, the cows have rather large hooves and when they try to flip a certain tile, they also flip all the adjacent tiles (tiles that share a full edge with the flipped tile). Since the flips are tiring, the cows want to minimize the number of flips they have to make.

Help the cows determine the minimum number of flips required, and the locations to flip to achieve that minimum. If there are multiple ways to achieve the task with the minimum amount of flips, return the one with the least lexicographical ordering in the output when considered as a string. If the task is impossible, print one line with the word "IMPOSSIBLE".

### Input

Line 1: Two space-separated integers: *M* and *N* 
Lines 2..*M*+1: Line *i*+1 describes the colors (left to right) of row i of the grid with *N* space-separated integers which are 1 for black and 0 for white

### Output

Lines 1..*M*: Each line contains *N* space-separated integers, each specifying how many times to flip that particular location.

### Sample Input

```
4 4
1 0 0 1
0 1 1 0
0 1 1 0
1 0 0 1
```

### Sample Output

```
0 0 0 0
1 0 0 1
1 0 0 1
0 0 0 0
```

## Solution

### AC代码

```c++
#define MAXN 17
const int dx[5] = {-1, 0, 0, 0, 1};
const int dy[5] = {0, -1, 0, 1, 0};

int M, N;
int tile[MAXN][MAXN];
int opt[MAXN][MAXN];
int flip[MAXN][MAXN];

int getcolor(int x, int y)
{
    int c = tile[x][y];
    for(int d=0; d<5; d++)
    {
        int x2 = x+dx[d], y2 = y+dy[d];
        if(x2>=0 && x2<M && y2>=0 && y2<N)
            c += flip[x2][y2];
    }
    return c&1;
}

int calc()
{
    for(int i=1; i<M; i++)
        for(int j=0; j<N; j++)
            if(getcolor(i-1, j))
                flip[i][j] = 1;

    for(int j=0; j<N; j++)
        if(getcolor(M-1, j))
            return -1;

    int ans = 0;
    for(int i=0; i<M; i++)
        for(int j=0; j<N; j++)
            ans += flip[i][j];
    return ans;
}

void solve()
{
    int res = -1;

    for(int i=0; i < 1<<N; i++)
    {
        memset(flip, 0, sizeof(flip));

        for(int j=0; j<N; j++)
            flip[0][N-j-1] = i>>j&1;

        int num = calc();
        if(num >= 0 && (res<0 || res > num))
        {
            res = num;
            memcpy(opt, flip, sizeof(flip));
        }
    }

    if(res < 0)
        printf("IMPOSSIBLE\n");
    else
    {
        for(int i=0; i<M; i++)
            for(int j=0; j<N; j++)
        printf("%d%c", opt[i][j], " \n"[j+1 == N]);
    }
}
int main()
{
    scanf("%d%d", &M, &N);
    for(int i=0; i<M; i++)
        for(int j=0; j<N; j++)
            scanf("%d", &tile[i][j]);
    solve();
    return 0;
}
```

