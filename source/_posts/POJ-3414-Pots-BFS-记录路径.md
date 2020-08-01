---
title: POJ 3414 Pots(BFS+记录路径)
date: 2018-08-15 16:00:26
tags: kuangbin带你飞
categories: [ACM, BFS]
---

传送门：[Pots](http://poj.org/problem?id=3414)

<blockquote class="blockquote-center">人一我百！人十我万！永不放弃~~~ 怀着自信的心，去追逐梦想 ——kuangbin </blockquote>

## Problem

###  Description

You are given two pots, having the volume of **A** and **B** liters respectively. The following operations can be performed:

1. FILL(i)        fill the pot **i** (1 ≤ **i** ≤ 2) from the tap;
2. DROP(i)      empty the pot **i** to the drain;
3. POUR(i,j)    pour from pot **i** to pot **j**; after this operation either the pot **j** is full (and there may be some water left in the pot **i**), or the pot **i** is empty (and all its contents have been moved to the pot **j**).

Write a program to find the shortest possible sequence of these operations that will yield exactly **C** liters of water in one of the pots.<!--more-->

### Input

On the first and only line are the numbers **A**, **B**, and **C**. These are all integers in the range from 1 to 100 and **C**≤max(**A**,**B**).

### Output

The first line of the output must contain the length of the sequence of operations **K**. The following **K** lines must each describe one operation. If there are several sequences of minimal length, output any one of them. If the desired result can’t be achieved, the first and only line of the file must contain the word ‘**impossible**’.

### Sample Input

```
3 5 4
```

### Sample Output

```
6
FILL(2)
POUR(2,1)
DROP(1)
POUR(2,1)
FILL(2)
POUR(2,1)
```
## Solution

### 题意

这是一道走马分油的题目，要求路径回溯，觉得挺好玩的，写了下。

### AC代码

```c++
#define MAXN　210
int a, b, c;
bool vis[MAXN][MAXN];

//这是一道走马分油的题，有意思
struct node
{
    int pre, id, po1, po2, op, op1, op2, step;
};
vector<node> arr;
int bfs()
{
    queue<node> que;
    node cur = {0, 0, 0, 0, 0, 0, 0, 0};
    que.push(cur);
    arr.push_back(cur);
    vis[0][0] = true;
    int p1, p2, id, cnt = 1, step;

    while(!que.empty())
    {
        cur = que.front();

        que.pop();
        //cout << "Pre:" << arr[cur.pre].po1 << " " << arr[cur.pre].po2  << " Op: " << cur.op << cur.op1 << cur.op2 << endl << "Now: " << cur.po1 << " " << cur.po2 << endl;
        //getchar();
        if(cur.po1 == c || cur.po2 == c)
            return cur.id;

        p1 = cur.po1;
        p2 = cur.po2;
        id = cur.id;
        step = cur.step;


        if(!vis[a][p2])
        {
            vis[a][p2] = true;
            node todo = {id, cnt++, a, p2, 1, 1, 0, step+1};
            que.push(todo);
            arr.push_back(todo);
        }
        if(!vis[p1][b])
        {
            vis[p1][b] = true;
            node todo = {id, cnt++, p1, b, 1, 2, 0, step+1};
            que.push(todo);
            arr.push_back(todo);
        }
        if(!vis[0][p2])
        {
            vis[0][p2] = true;
            node todo = {id, cnt++, 0, p2, 2, 1, 0, step+1};
            que.push(todo);
            arr.push_back(todo);
        }
        if(!vis[p1][0])
        {
            vis[p1][0] = true;
            node todo = {id, cnt++, p1, 0, 2, 2, 0, step+1};
            que.push(todo);
            arr.push_back(todo);
        }
        int cur1, cur2;
        //pot1 -> pot2
        cur1 = p1-(b-p2);
        if(cur1 < 0)
            cur1 = 0;
        cur2 = p2+p1;
        if(cur2 > b)
            cur2 = b;

        if(!vis[cur1][cur2])
        {
            vis[cur1][cur2] = true;
            node todo = {id, cnt++, cur1, cur2, 3, 1, 2, step+1};
            que.push(todo);
            arr.push_back(todo);
        }

        //pot2 -> pot1
        cur2 = p2 - (a-p1);
        if(cur2 < 0)
            cur2 = 0;
        cur1 = p1+p2;
        if(cur1 > a)
            cur1 = a;

        if(!vis[cur1][cur2])
        {
            vis[cur1][cur2] = true;
            node todo = {id, cnt++, cur1, cur2, 3, 2, 1, step+1};
            que.push(todo);
            arr.push_back(todo);
        }
    }

    return -1;
}
bool cmp(node n1, node n2)
{
    return n1.id < n2.id;
}
void solve()
{
    //sort(arr.begin(), arr.end(), cmp);
    stack<node> st;
    int last = bfs();
    int cnt = 0;
    while(last != 0 && last != -1)
    {
        st.push(arr[last]);
        last = arr[last].pre;
        cnt++;
    }

    if(last == -1)
    {
        puts("impossible");
        return;
    }
    printf("%d\n", cnt);
    node cur;
    while(!st.empty())
    {
        cur = st.top();
        st.pop();
        if(cur.op == 1)
            printf("FILL(%d)\n", cur.op1);
        else if(cur.op == 2)
            printf("DROP(%d)\n", cur.op1);
        else if(cur.op == 3)
            printf("POUR(%d,%d)\n", cur.op1, cur.op2);
    }
}
int main()
{
    scanf("%d%d%d", &a, &b, &c);
    solve();
    return 0;
}
```

## Note

敲代码时不要轻易复制TAT，不要因为下边要写的和上边的一样就复制= =，吃亏了呀。

写的时候还是纠结了下怎么回溯。

最初判断 impossble 的时候考虑不周，会WA或RE。

为了避免麻烦， = =， 没有优化BFS。