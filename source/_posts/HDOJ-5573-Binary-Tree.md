---
title: HDOJ 5573 Binary Tree
date: 2018-07-13 21:41:44
tags: 算法
categories: [ACM, 思维]
---

[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=5573)

![HDU 5573](http://cmhblog.cfzhao.com/18-7-13/45119042.jpg)<!--more-->

 ## 题意

&emsp;&emsp;在一棵以 1 为根的满二叉树上，然后从根节点到第k层的某一个结点，你可以以一些途径到达，然后经过的节点编号需要加加减减，问你怎么凑出n，特判数据。

## 思路

&emsp;&emsp;一开始想到的是 dfs 深搜， 可没能想出可行的剪枝函数，之后转向了新的思路。若每层都取 + 号，到达第 K 层时，其和为 $2^k-1$ 到 $2^{k-1}-n-2$ 之间， 也就是如果我们一直走左边， 得到的最大值为 $2^k-1$，如果我们只最后一步走右边（下边的思路所说的路径都是除最后一步外只往左走），则为 $2^k$ ，由**题目数据范围**可知， $N\le2^k$ ，即我们如果一直走左边的话是可以取到最大值的。最左边一枝的编号为  $2^k$ ，联想到数的二进制表示，即我们可以取路径上边的部分或全部节点构成任意 1 ~ $2^k$ 的数字。题意可知，我们必须要走 K 层，如果选取部分节点，那剩下的结点怎么办呢。思路一下僵结住，不知道该如何向下进行。后来灵光一闪，想到可以反过来用 $2^k$ 减去某个数得到我们想要的 $N$ ，这题便出来了。

## 解法

1. $diff = \frac{2^k-n}{2}$
2. N 若为奇数，则最后一步向左走；若为偶数，则向右走。将符号全部初始为 + 。
3. 求出 $diff$ 的二进制，其二进制位若为1，则将其对应位的符号改为 - 。

## 代码

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
#define MAXN 65
#define INF 0x3f3f3f3f
#define DEBUG
#define DataIn
typedef long long LL;

using namespace std;

int T, k;
LL n;
LL maxx;
LL tar;
bool sub[MAXN];

int main()
{
    scanf("%d", &T);
    for(int kiss = 1; kiss <= T; kiss++)
    {
        scanf("%lld%d", &n, &k);
        memset(sub, false, sizeof(sub));
        maxx = 1LL << k;
        tar = (maxx - n) >> 1;
        int cnt = 1;
        while(tar)
        {
            if(tar & 1)
            {
                sub[cnt] = true;
            }
            tar >>= 1;
            cnt++;
        }

        printf("Case #%d:\n", kiss);
        for(int i=1; i<=k-1; i++)
        {
            printf("%lld %c\n", 1LL<<(i-1), sub[i] ? '-' : '+');
        }

        if(n & 1)
            printf("%lld %c\n", 1LL<<(k-1), sub[k] ? '-' : '+');
        else
            printf("%lld %c\n", (1LL<<(k-1))+1, sub[k] ? '-' : '+');
    }

}
```

![AC](http://cmhblog.cfzhao.com/blog/180713/1A7fK73gE9.png)

![mark](http://cmhblog.cfzhao.com/blog/180713/kbmJK514K8.png)

## 心得

&emsp;&emsp;刷题时要仔细看题目给的数据范围，这题因为没有注意 $N\le2^k$ 纠结了很久，注意到后突然醒悟可以只走左边构成该值。

&emsp;&emsp;要多思考，当得出思路，一次 AC 的时候真的很喜悦，很有成就感！