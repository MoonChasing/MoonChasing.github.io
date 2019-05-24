---
title: POJ 3126 Prime Path(素数表+BFS)
date: 2018-08-15 15:45:30
tags: kuangbin带你飞
categories: [ACM, BFS]
---

传送门：[Prime Path](http://poj.org/problem?id=3126)

<blockquote class="blockquote-center">人一我百！人十我万！永不放弃~~~ 怀着自信的心，去追逐梦想 ——kuangbin </blockquote>

## Problem

### Description

The ministers of the cabinet were quite upset by the message from the Chief of Security stating that they would all have to change the four-digit room numbers on their offices. <!--more-->

— It is a matter of security to change such things every now and then, to keep the enemy in the dark. 

— But look, I have chosen my number 1033 for good reasons. I am the Prime minister, you know! 

— I know, so therefore your new number 8179 is also a prime. You will just have to paste four new digits over the four old ones on your office door. 

— No, it’s not that simple. Suppose that I change the first digit to an 8, then the number will read 8033 which is not a prime! 

— I see, being the prime minister you cannot stand having a non-prime number on your door even for a few seconds. 

— Correct! So I must invent a scheme for going from 1033 to 8179 by a path of prime numbers where only one digit is changed from one prime to the next prime. 

Now, he minister of finance, who had been eavesdropping, intervened. 

— No unnecessary expenditure, please! I happen to know that the price of a digit is one pound. 

— Hmm, in that case I need a computer program to minimize the cost. You don't know some very cheap software gurus, do you? 

— In fact, I do. You see, there is this programming contest going on... Help the prime minister to find the cheapest prime path between any two given four-digit primes! The first digit must be nonzero, of course. Here is a solution in the case above. 

> 1033
> 1733
> 3733
> 3739
> 3779
> 8779
> 8179

The cost of this solution is 6 pounds. Note that the digit 1 which got pasted over in step 2 can not be reused in the last step – a new 1 must be purchased.

### Input

One line with a positive number: the number of test cases (at most 100). Then for each test case, one line with two numbers separated by a blank. Both numbers are four-digit primes (without leading zeros).

### Output

One line for each case, either with a number stating the minimal cost or containing the word Impossible.

### Sample Input

```
3
1033 8179
1373 8017
1033 1033
```

### Sample Output

```
6
7
0
```

## Solution

### 题意

给两个四位素数，每次只能改变其中一位，改后的数字也要求是素数，问最小的改变次数。

### AC代码

```C++
#define MAXN 10007
bool notprime[MAXN];
bool vis[MAXN];
int n, m;
typedef struct node
{
    int num, step;
}P;

void init()
{
    memset(notprime, false, sizeof(notprime));
    notprime[0] = notprime[1] = true;
    for(int i=2; i<MAXN; i++)
    {
        if(!notprime[i])
        {
            if(i > MAXN/i)
                continue;
            for(int j=i*i; j<MAXN; j+=i)
                notprime[j] = true;
        }
    }
}

int bfs()
{
    int tmp, num, step;
    queue<P> que;

    que.push(P {n, 0});
    vis[n] = true;

    while(!que.empty())
    {
        num = que.front().num;
        step = que.front().step;
        if(num == m)
            return step;
        que.pop();

        for(int i=0; i<10; i++)
        {
            tmp = num / 10 * 10 + i;
            if(tmp == m)
                return step+1;
            if(!notprime[tmp] && !vis[tmp])
                que.push(P {tmp, step+1}), vis[tmp] = true; //辣鸡崔明浩，最初又忘了改变状态
        }
        for(int i=0; i<10; i++)
        {
            tmp = num/100*100 + num%10 + i*10;
            if(tmp == m)
                return step+1;
            if(!notprime[tmp] && !vis[tmp])
                que.push(P {tmp, step+1}), vis[tmp] = true;
        }
        for(int i=0; i<10; i++)
        {
            tmp = num/1000*1000 + num%100 + i*100;
            if(tmp == m)
                return step+1;
            if(!notprime[tmp] && !vis[tmp])
                que.push(P {tmp, step+1}), vis[tmp] = true;
        }
        for(int i=1; i<10; i++)
        {
            tmp = i*1000 + num%1000;
            if(tmp == m)
                return step+1;
            if(!notprime[tmp] && !vis[tmp])
                que.push(P {tmp, step+1}), vis[tmp] = true;
        }
    }
    return -1;
}
int main()
{
    int T;
    scanf("%d", &T);
    init();
    int ans;
    while(T--)
    {
        memset(vis, false, sizeof(vis));
        scanf("%d%d", &n, &m);

        ans = bfs();
        if(~ans)
            printf("%d\n", ans);
        else
            printf("Impossible\n");	//辣鸡崔明浩，一开始没写Impossible
    }
    return 0;
}
```

