---
title: FZU 2150 Fire Game(BFS)
date: 2018-08-15 18:14:07
tags: kuangbin带你飞
categories: [ACM, BFS]
---

传送门：[Fire Game](https://vjudge.net/contest/244906#problem/I)

<blockquote class="blockquote-center">人一我百！人十我万！永不放弃~~~ 怀着自信的心，去追逐梦想 ——kuangbin </blockquote>

## Problem

### Description

Fat brother and Maze are playing a kind of special (hentai) game on an N*M board (N rows, M columns). At the beginning, each grid of this board is consisting of grass or just empty and then they start to fire all the grass. Firstly they choose two grids which are consisting of grass and set fire. As we all know, the fire can spread among the grass. If the grid (x, y) is firing at time t, the grid which is adjacent to this grid will fire at time t+1 which refers to the grid (x+1, y), (x-1, y), (x, y+1), (x, y-1). This process ends when no new grid get fire. If then all the grid which are consisting of grass is get fired, Fat brother and Maze will stand in the middle of the grid and playing a MORE special (hentai) game. (Maybe it’s the OOXX game which decrypted in the last problem, who knows.)![](https://www.lifeofpix.com/wp-content/uploads/2018/07/pink-mountains-1600x1067.jpg)<!--more-->

You can assume that the grass in the board would never burn out and the empty grid would never get fire.

Note that the two grids they choose can be the same.

### Input

The first line of the date is an integer T, which is the number of the text cases.

Then T cases follow, each case contains two integers N and M indicate the size of the board. Then goes N line, each line with M character shows the board. “#” Indicates the grass. You can assume that there is at least one grid which is consisting of grass in the board.

1 <= T <=100, 1 <= n <=10, 1 <= m <=10

### Output

For each case, output the case number first, if they can play the MORE special (hentai) game (fire all the grass), output the minimal time they need to wait after they set fire, otherwise just output -1. See the sample input and output for more details.

### Sample Input

```
4
3 3
.#.
###
.#.
3 3
.#.
#.#
.#.
3 3
...
#.#
...
3 3
###
..#
#.#
```

### Sample Output

```
Case 1: 1
Case 2: -1
Case 3: 0
Case 4: 2
```

## Solution

### AC代码

```c++
#define MAXN 13
#define INF 0x3f3f3f3f
int T, n, m, sum;
char mz[MAXN][MAXN];
bool vis[MAXN][MAXN];

const int dx[4] = {0, 0, -1, 1};
const int dy[4] = {-1, 1, 0, 0};

typedef struct point
{
    int x, y, t;
    point() {}
    point(int _x, int _y, int _t)
    {
        x = _x, y = _y, t = _t;
    }
} P;

int bfs(int x, int y)   //返回在x， y两点放火所需的时间
{
    int cnt = 0;
    int curx1 = x/m, cury1 = x-x/m*m;
    int curx2 = y/m, cury2 = y-y/m*m;

    queue<P> que;
    P p;

    que.push(P(curx1, cury1, 0));
    vis[curx1][cury1] = true;

    que.push(P(curx2, cury2, 0));
    vis[curx2][cury2] = true;
    if(x == y)
        cnt++;
    else
        cnt += 2;
    if(cnt == sum)
        return 0;
    while(!que.empty())
    {
        p = que.front();
        que.pop();

        curx1 = p.x;
        cury1 = p.y;

        for(int d=0; d<4; d++)
        {
            curx2 = curx1 + dx[d];
            cury2 = cury1 + dy[d];
            if(curx2 >= 0 && curx2 < n && cury2 >= 0 && cury2 < m && !vis[curx2][cury2] && mz[curx2][cury2] == '#')
            {
                vis[curx2][cury2] = true;
                que.push(P(curx2, cury2, p.t+1));
                cnt++;
                if(cnt == sum)
                    return p.t+1;
            }
        }
    }
    return INF;
}

int solve()
{
    int sz  = n*m;
    int ans = INF;
    for(int i=0; i<sz; i++)
    {
        if(mz[i/m][i-i/m*m] == '.')
            continue;
        for(int j=i; j<sz; j++)
        {
            if(mz[j/m][j-j/m*m] == '.')
                continue;
            memset(vis, false, sizeof(vis));
            ans = min(ans, bfs(i, j));
        }
    }
    return ans;
}

int main()
{
    scanf("%d", &T);
    for(int kiss = 1; kiss<=T; kiss++)
    {
        scanf("%d%d", &n, &m);
        getchar();
        sum = 0;
        for(int i=0; i<n; i++)
        {
            for(int j=0; j<m; j++)
            {
                mz[i][j] = getchar();
                if(mz[i][j] == '#')
                    sum++;
            }
            getchar();
        }

        int ans = solve();
        printf("Case %d: %d\n", kiss, ans==INF ? -1 : ans);
    }
    return 0;
}
```

