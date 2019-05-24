---
title: BZOJ 5486 Fine Dining
date: 2019-05-19 23:37:07
tags: [最短路]
---



传送门：[BZOJ 5486 Fine Dining](https://www.lydsy.com/JudgeOnline/problem.php?id=5486)

# Problem

## Description

漫长的一天结束了，饥困交加的奶牛们准备返回牛棚。农场由N片牧场组成（2≤N≤50,000），方便起见编号为1…

N。所有奶牛都要前往位于牧场N的牛棚。其他N-1片牧场中每片有一头奶牛。奶牛们可以通过M条无向的小路在牧场之间移动（1≤M≤100,000）。第i条小路连接牧场ai和bi，通过需要时间ti。每头奶牛都可以经过一些小路回到牛棚。由于饥饿，奶牛们很乐于在他们回家的路上停留一段时间觅食。农场里有K个有美味的干草捆，第i个干草捆的美味值为yi。每头奶牛都想要在她回牛棚的路上在某一个干草捆处停留，但是她这样做仅当经过这个干草捆使她回牛棚的时间增加不超过这个干草捆的美味值。注意一头奶牛仅仅“正式地”在一个干草捆处因进食而停留，即使她的路径上经过其他放有干草捆的牧场；她会简单地无视其他的干草捆。

## Input

第一行包含三个空格分隔的整数N，M和K。

以下M行每行包含三个整数ai，bi和ti，表示牧场ai和bi之间有一条需要ti时间通过的小路

（ai不等于bi，ti为不超过104的正整数）。

以下K行，每行以两个整数描述了一个干草捆：该干草捆所在牧场的编号，以及它的美味值（一个不超过10^9的正整数）。

同一片牧场中可能会有多个干草捆。

## Output

输出包含N-1行。

如果牧场i里的奶牛可以在回牛棚的路上前往某一个干草捆并且在此进食，则第i行为一个整数1，否则为一个整数0

## Sample Input

4 5 1
 1 4 10
 2 1 20
 4 2 3
 2 3 5
 4 3 2
 2 7

## Sample Output

1
 1
 1
 在这个例子中，牧场3里的奶牛可以停留进食，因为她回去的时间仅会增加6（从2增加到8），而这个增加量并没有
 超过干草捆的美味值7。牧场2里的奶牛显然可以吃掉牧场2里的干草，因为这并不会导致她的最佳路径发生变化。
 对于牧场1里的奶牛是一个有趣的情况，因为看起来她的最佳路径（10）可能会因为前去进食而有过大的增加量。
 然而，确实存在一条路径使得她可以前去干草捆处停留：先到牧场4，然后去牧场2（吃草），然后回到牧场4。 

# Solution

# Idea

分层图最短路。两种状态，吃或未吃干草。$dis[i] [0]$表示未吃干草状态下到达i的最短时间，$dis[i][1]$则为吃干草状态下到达i的最短时间。

**将牧场之间的距离看做为正，将干草看为负**。如果$dis[n][1] <= dis[n][0]$ ，则输出1.

## Code

```C++
#define MAXN 50050
#define MAXM 200050
#define INF 0x3f3f3f3f
typedef long long LL;

using namespace std;

int n, m, k;

struct ed
{
    int to, next, cost;
}edge[MAXM];

int tot;
int head[MAXN];
int has[MAXN], dis[MAXN][2];
bool vis[MAXN];

void addedge(int u, int v, int w)
{
    edge[tot].to = v;
    edge[tot].next = head[u];
    edge[tot].cost = w;
    head[u] = tot++;
}

void spfa()
{
    memset(dis, 0x3f, sizeof(dis));
    dis[n][0] = 0;
    queue<int> que;
    que.push(n);
    while(!que.empty())
    {
        int u = que.front();
        int v;
        que.pop();
        vis[u] = false;

        for(int i = head[u]; ~i; i = edge[i].next)
        {
            v = edge[i].to;
            if(dis[u][0] + edge[i].cost < dis[v][0])
            {
                dis[v][0] = dis[u][0] + edge[i].cost;
                if(!vis[v])
                {
                    que.push(v);
                    vis[v] = true;
                }
            }

            if(has[v] && dis[u][0] + edge[i].cost - has[v] < dis[v][1])
            {
                dis[v][1] = dis[u][0] + edge[i].cost - has[v];
                if(!vis[v])
                {
                    que.push(v);
                    vis[v] = true;
                }
            }

            if(dis[u][1] + edge[i].cost < dis[v][1])
            {
                dis[v][1] = dis[u][1] + edge[i].cost;
                if(!vis[v])
                {
                    que.push(v);
                    vis[v] = true;
                }
            }
        }
    }
}

int main()
{
    tot = 0;
    memset(head, -1, sizeof(head));

    scanf("%d%d%d", &n, &m, &k);
    int u, v, w;

    for(int i=0; i<m; i++)
    {
        scanf("%d%d%d", &u, &v, &w);
        addedge(u, v, w);
        addedge(v, u, w);
    }
    for(int i=0; i<k; i++)
    {
        scanf("%d%d", &u, &w);
        has[u] = max(has[u], w);
    }

    if(has[n])
    {
        for(int i=1; i<n; i++)
            puts("1");
        return 0;
    }
    spfa();
    for(int i=1; i<=n-1; i++)
    {
        if(dis[i][0] >= dis[i][1])
            puts("1");
        else
            puts("0");
    }

    return 0;
}
```

