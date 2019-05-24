---
title: POJ 3278 Catch That Cow(BFS+剪枝)
date: 2018-08-11 11:47:30
tags: kuangbin带你飞
categories: [ACM, BFS]
---

<blockquote class="blockquote-center">人一我百！人十我万！永不放弃~~~ 怀着自信的心，去追逐梦想 ——kuangbin </blockquote>

传送门：[POJ 3278 Catch That Cow](http://poj.org/problem?id=3278)

## Problem

### Description

Farmer John has been informed of the location of a fugitive cow and wants to catch her immediately. He starts at a point *N* (0 ≤ *N* ≤ 100,000) on a number line and the cow is at a point *K* (0 ≤ *K* ≤ 100,000) on the same number line. Farmer John has two modes of transportation: walking and teleporting.<!--more-->

\* Walking: FJ can move from any point *X* to the points *X* - 1 or *X* + 1 in a single minute
\* Teleporting: FJ can move from any point *X* to the point 2 × *X* in a single minute.

If the cow, unaware of its pursuit, does not move at all, how long does it take for Farmer John to retrieve it?

### Input

Line 1: Two space-separated integers: *N* and *K*

### Output

Line 1: The least amount of time, in minutes, it takes for Farmer John to catch the fugitive cow.

### Sample Input

```
5 17
```

### Sample Output

```
4
```

### Hint

The fastest way for Farmer John to reach the fugitive cow is to move along the following path: 5-10-9-18-17, which takes 4 minutes.

## Solution

### AC代码

```c++
#define MAXN 100010
int n, k;
typedef struct point
{
    int pos, step;
} P;

int minn = INF;
int ans = INF;
bool vis[MAXN];
void bfs()
{
    queue<P> que;
    que.push(P {n, 0});
    vis[n] = true;
    P cur;
    while(!que.empty())
    {
        cur = que.front();
        que.pop();
        if(cur.pos < k)
        {
            if(cur.pos-1>=0 && !vis[cur.pos-1])
            {
                que.push(P {cur.pos-1, cur.step+1});
                vis[cur.pos-1] = true;
            }
            if(cur.pos+1<MAXN && !vis[cur.pos+1])
                que.push(P {cur.pos+1, cur.step+1}), vis[cur.pos+1] = true;
            if(cur.pos * 2 < MAXN && !vis[cur.pos << 1])
            {
                if(cur.pos * 2 <= k)
                    que.push(P {cur.pos<<1, cur.step+1});
                else
                {
                    if(cur.step + 1 + cur.pos * 2 - k <= ans)
                        ans = cur.step + 1 + cur.pos * 2 - k;
                }
                vis[cur.pos << 1] = true;
            }

        }
        else if(cur.pos == k)
        {
            ans = min(ans, cur.step);
        }
        else if(cur.pos > k)
        {
            ans = min(ans, cur.step+cur.pos-k);
        }
    }
}
int main()
{
    scanf("%d%d", &n, &k);

    if(k <= n)
        ans = n-k;
    else
        bfs();

    printf("%d\n",ans);
    return 0;
}
```

## Note

这个地球真太小了，在地铁上看小米的面经写到了一个算法题，心想挺有意思的，回去写写。回家继续写 kuangbin带你飞 的题目，结果发现第三道刚好是面经中看到的。太小了太小了。简单题目。哈哈哈，看到 Teleporting 的时候第一反应是 TP。