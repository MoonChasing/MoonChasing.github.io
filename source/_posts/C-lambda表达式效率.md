---
title: C++ lambda表达式效率
date: 2019-03-01 15:16:19
tags: [C++, lambda]
categories: C++
---

今天看代码的时候看到有人在 sort 里用了 lambda 表达式，好奇心爆棚的我就寻思这样用的效率和我们定义一个函数之后调用这个函数两种方法哪个效率高。

于是自己动手比较了下，因为之前从没有用过 lambda 表达式，所以这也算是自己第一次使用 lambda 了。

#### 比较代码

```C++
#include<cstdio>
#include<algorithm>
#include<vector>
#include<ctime>
#include<cstdlib>
using namespace std;

const int MAXN = 1e6+7;
bool cmp(const int &a, const int &b)
{
    return a<b;
}
int main()
{
    srand((unsigned int)time(NULL));
    vector<int> vec(MAXN);
    int time1 = 0;
    for(int cnt=0; cnt<5; cnt++)
    {
        for(int i=0; i<MAXN; i++)
            vec[i] = rand();
        int start = clock();
        sort(vec.begin(), vec.end(), cmp);
        time1 += clock() - start;
    }

    int time2 = 0;

    for(int cnt=0; cnt<5; cnt++)
    {
        for(int i=0; i<MAXN; i++)
            vec[i] = rand();
        int start = clock();
        sort(vec.begin(), vec.end(), [](const int &a, const int &b){
                return a < b;
             });
        time2 += clock() - start;
    }
    printf("%d %d\n", time1, time2);
    return 0;
}
```

#### 输出

多次实验后， time1(不用 lambda 方法)时间大约为 2050 ms左右，而 time2 （使用 lambda 方法）耗时大约为 2250 ms 左右。所以估计 lambda 表达式效率会低一丢丢。