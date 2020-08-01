---
title: LeetCode 41. First Missing Positive
date: 2018-08-20 09:55:24
tags: 
Categories: LeetCode
---

<blockquote class="blockquote-center">我失去了一只臂膀， 就睁开了一只眼睛。 <br>——顾城</blockquote>

## Problem

Given an unsorted integer array, find the smallest missing positive integer.<!--more-->

**Example 1:**

```
Input: [1,2,0]
Output: 3
```

**Example 2:**

```
Input: [3,4,-1,1]
Output: 2
```

**Example 3:**

```
Input: [7,8,9,11,12]
Output: 1
```

**Note:**

Your algorithm should run in *O*(*n*) time and uses constant extra space.

 ## Solution

**LeetCode第一题** 

### 思路

打ACM被血虐，无聊来看看LeetCode，顺手选了一个Hard的题目。题意很简单，不像ACM一样嘟噜嘟噜说很多。第一反应有些懵，要求O(n)的时间复杂度，和O(1)的空间。但很快灵光一闪想到O(n)时间复杂度的排序后，就有了思路。数列中n个数，最优情况下值分别为1~n，这样我们可以分别把数列中的数字放到自己对应的下标中去(我向vector中额外加了一个数，这样子就可以对应的下标放对应的数)，遇到大于n或小于0的数就不进行操作。处理完之后开始扫一遍数组，(因为我加了一个数，所以从下标为1的位置开始扫到n就好了)，第1个值和下标不对应的数就是答案。如果全都对应，那答案为n+1。

一开始本来想用迭代完成对数组的处理(想以最优的解法来处理，减少数字变动的次数)，结果写崩了，思路不是太流畅。遂用了递归实现，结果发现用了4ms，仅击败了80%左右的答案。又开始优化，虽然比一开始的想法差点，数字交换次数会多一些，但代码很简洁，代码量极少。而且仅用了0ms，为最优答案。

### AC代码 Ver1

```c++
class Solution
{
public:
    int n;
    void f(int pos, int foo, vector<int>& a)
    {
        int val = a[pos];
        if(val == pos)
            return;
        if(val < 0 || val > n)
        {
            a[pos] = pos;
            return;
        }
        else
        {
            a[pos] = foo;
            f(val, val, a);
        }
    }

    int firstMissingPositive(vector<int>& nums)
    {
        int cur=0, temp, pre, now;
        n = nums.size();
        nums.push_back(-1);

        while(cur <= n)
        {
            temp = nums[cur];
            if(temp == cur)
            {
                cur++;
                continue;
            }
            else if(temp > n || temp < 0)
            {
                //nums[cur++] = -1;
                cur++;
                continue;
            }
            else
            {
                f(cur, nums[cur], nums);
                cur++;
            }
        }

        for(int i=1; i<=n; i++)
        {
            if(nums[i] != i)
                return i;
        }
        return n+1;
    }
};
```

![mark](http://cmhblog.cfzhao.com/blog/180820/DmjA8h22G1.png)

### AC代码 Ver2

```c++
class Solution
{
public:

    int firstMissingPositive(vector<int>& nums)
    {
        int n = nums.size();
        nums.push_back(-1);

        for(int i=0; i<=n; i++)
            while(nums[i] >= 0 && nums[i]<=n && nums[nums[i]] != nums[i])
            {
                swap(nums[i], nums[nums[i]]);
                //nums[i] ^= nums[nums[i]];
                //nums[nums[i]] ^= nums[i];
                //nums[i] ^= nums[nums[i]];
            }

        for(int i=1; i<=n; i++)
            if(nums[i] != i)
                return i;
        return n+1;
    }
};
```

![mark](http://cmhblog.cfzhao.com/blog/180820/ima82mh808.png)

## Note

自己LeetCode第一题1A，还是很开心的。一开始打ACM心情不太好，看这题过了突然高兴起来。

递归速度要比迭代慢一些，而且有一定的空间消耗。

**小心用异或来交换值**，比如这题自己写了

```c++
nums[i] ^= nums[nums[i]];
nums[nums[i]] ^= nums[i];
nums[i] ^= nums[nums[i]];
```

结果却报了错，自己懵了许久。过了一会才恍然大悟，因为`nums[nums[i]]`中使用了`nums[i]`作下标， 而`nums[i]`的值在之前的异或中已经更换了值= =。

好像`std::swap()`函数交换要快一些，自己用异或写的函数竟然会4ms，而`swap()`实现的版本只要0ms。