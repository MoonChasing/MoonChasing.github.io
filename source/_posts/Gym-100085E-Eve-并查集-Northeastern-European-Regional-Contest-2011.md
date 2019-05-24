---
title: Gym 100085E Eve(并查集)(Northeastern European Regional Contest 2011)
date: 2018-08-23 22:33:36
tags:
categories: [ACM, 数据结构, 并查集]
---

传送门:[Northeastern European Regional Contest 2011 E. Eve](https://odzkskevi.qnssl.com/2cabff6f0a36e98d56356562c5755389?v=1534601556)

## Problem

<p align="center">Input file: eve.in<br>Output file: eve.out</p>
Mitochondrial DNA is the Deoxyribonucleic acid molecule that is contained in mitochondria within cellsof an organism.
Mitochondrial DNA is normally passed to a child exclusively from its mother. Because of this fact, it is
possible to speak of “Mitochondrial Eve” which refers to the most recent common matrilineal ancestor
of the entire population. Matrilineal ancestry is traced through female line: mother, grandmother, etc.
Mitochondrial Eve of the Earth’s human population is estimated to have lived around 200 000 years ago,
most likely in the East Africa.![](https://www.lifeofpix.com/wp-content/uploads/2018/04/IMG_4475-1600x900.jpg)<!--more-->

In this problem, we consider a certain population of the same biological species (eukaryotic and anisogamous).
The population has been observed for a period of time, and all births and deaths were clearly
recorded. For some of the individuals within the population, their mitochondrial DNA was sequenced.
It is assumed that each individual in the observed population received its mitochondrial DNA from its
mother without any mutations.
Your task is to find out, whether all individuals currently alive have the same mitochondrial DNA.
Input
The first line of the input file contains integer n (1 ≤ n ≤ 100 000), the number of individuals that were
alive at the beginning of the observation period. IDs of these individuals are integers from 1 to n.
Next n lines contain one character each. The i-th of these lines describes the sex of the individual with
ID i. ‘M’ stands for male, ‘F’ stands for female.
The next line contains integer m (0 ≤ m ≤ 100 000), the total number of births and deaths that happened
during the observation period.
Next m lines contain description of birth and death events, listed in chronological order.
A birth event is described by three space-separated tokens: the ID of the father, the ID of the mother,
and a character that describes the sex of the child (‘M’ for male, ‘F’ for female). The ID given to the
offspring is the smallest positive integer that hasn’t been used as an ID by this point of time.
A death event is described by a single negative integer, whose absolute value equals the ID of the individual
that died.
The next line contains integer k (0 ≤ k ≤ n + m), the number of sequenced mitochondrial DNAs.
Each of the next k lines contains two space-separated integers, the ID of the individual whose mitochondrial
DNA has been sequenced, and the unique identifier of the mitochondrial DNA of that individual.
Unique identifiers of two mitochondrial DNAs are the same if and only if the corresponding sequenced
mitochondrial DNAs are the same. All unique identifiers of the mitochondrial DNAs are integers from 1
to 109
.
All the data given in the input file is consistent and non-contradictory. Each individual’s mitochondrial
DNA was sequenced at most once. At least one individual was alive at the end of the observation period.
Output
The output file must contain a single word:
• YES – if it can be derived that all the individuals that are alive at the end of the experiment have
the same mitochondrial DNA;
• NO – if it can be derived that some of the individuals that are alive at the end of the experiment
have different mitochondrial DNA;
• POSSIBLY – if none of the cases above takes place.
of an organism.
Mitochondrial DNA is normally passed to a child exclusively from its mother. Because of this fact, it is
possible to speak of “Mitochondrial Eve” which refers to the most recent common matrilineal ancestor
of the entire population. Matrilineal ancestry is traced through female line: mother, grandmother, etc.
Mitochondrial Eve of the Earth’s human population is estimated to have lived around 200 000 years ago,
most likely in the East Africa.
In this problem, we consider a certain population of the same biological species (eukaryotic and anisogamous).
The population has been observed for a period of time, and all births and deaths were clearly
recorded. For some of the individuals within the population, their mitochondrial DNA was sequenced.
It is assumed that each individual in the observed population received its mitochondrial DNA from its
mother without any mutations.
Your task is to find out, whether all individuals currently alive have the same mitochondrial DNA.
Input
The first line of the input file contains integer n (1 ≤ n ≤ 100 000), the number of individuals that were
alive at the beginning of the observation period. IDs of these individuals are integers from 1 to n.
Next n lines contain one character each. The i-th of these lines describes the sex of the individual with
ID i. ‘M’ stands for male, ‘F’ stands for female.
The next line contains integer m (0 ≤ m ≤ 100 000), the total number of births and deaths that happened
during the observation period.
Next m lines contain description of birth and death events, listed in chronological order.
A birth event is described by three space-separated tokens: the ID of the father, the ID of the mother,
and a character that describes the sex of the child (‘M’ for male, ‘F’ for female). The ID given to the
offspring is the smallest positive integer that hasn’t been used as an ID by this point of time.
A death event is described by a single negative integer, whose absolute value equals the ID of the individual
that died.
The next line contains integer k (0 ≤ k ≤ n + m), the number of sequenced mitochondrial DNAs.
Each of the next k lines contains two space-separated integers, the ID of the individual whose mitochondrial
DNA has been sequenced, and the unique identifier of the mitochondrial DNA of that individual.
Unique identifiers of two mitochondrial DNAs are the same if and only if the corresponding sequenced
mitochondrial DNAs are the same. All unique identifiers of the mitochondrial DNAs are integers from 1
to 109
.
All the data given in the input file is consistent and non-contradictory. Each individual’s mitochondrial
DNA was sequenced at most once. At least one individual was alive at the end of the observation period.
Output
The output file must contain a single word:
• YES – if it can be derived that all the individuals that are alive at the end of the experiment have
the same mitochondrial DNA;
• NO – if it can be derived that some of the individuals that are alive at the end of the experiment
have different mitochondrial DNA;
• POSSIBLY – if none of the cases above takes place.

### Description

Mitochondrial DNA is the Deoxyribonucleic acid molecule that is contained in mitochondria within cells
of an organism.
Mitochondrial DNA is normally passed to a child exclusively from its mother. Because of this fact, it is
possible to speak of “Mitochondrial Eve” which refers to the most recent common matrilineal ancestor
of the entire population. Matrilineal ancestry is traced through female line: mother, grandmother, etc.
Mitochondrial Eve of the Earth’s human population is estimated to have lived around 200 000 years ago,
most likely in the East Africa.
In this problem, we consider a certain population of the same biological species (eukaryotic and anisogamous).
The population has been observed for a period of time, and all births and deaths were clearly
recorded. For some of the individuals within the population, their mitochondrial DNA was sequenced.
It is assumed that each individual in the observed population received its mitochondrial DNA from its
mother without any mutations.
Your task is to find out, whether all individuals currently alive have the same mitochondrial DNA.

### Input
The first line of the input file contains integer n (1 ≤ n ≤ 100 000), the number of individuals that were
alive at the beginning of the observation period. IDs of these individuals are integers from 1 to n.
Next n lines contain one character each. The i-th of these lines describes the sex of the individual with
ID i. ‘M’ stands for male, ‘F’ stands for female.
The next line contains integer m (0 ≤ m ≤ 100 000), the total number of births and deaths that happened
during the observation period.
Next m lines contain description of birth and death events, listed in chronological order.
A birth event is described by three space-separated tokens: the ID of the father, the ID of the mother,
and a character that describes the sex of the child (‘M’ for male, ‘F’ for female). The ID given to the
offspring is the smallest positive integer that hasn’t been used as an ID by this point of time.
A death event is described by a single negative integer, whose absolute value equals the ID of the individual
that died.
The next line contains integer k (0 ≤ k ≤ n + m), the number of sequenced mitochondrial DNAs.
Each of the next k lines contains two space-separated integers, the ID of the individual whose mitochondrial
DNA has been sequenced, and the unique identifier of the mitochondrial DNA of that individual.
Unique identifiers of two mitochondrial DNAs are the same if and only if the corresponding sequenced
mitochondrial DNAs are the same. All unique identifiers of the mitochondrial DNAs are integers from 1
to $10^9$.
All the data given in the input file is consistent and non-contradictory. Each individual’s mitochondrial
DNA was sequenced at most once. At least one individual was alive at the end of the observation period.

### Output
The output file must contain a single word:
• YES – if it can be derived that all the individuals that are alive at the end of the experiment have
the same mitochondrial DNA;
• NO – if it can be derived that some of the individuals that are alive at the end of the experiment
have different mitochondrial DNA;
• POSSIBLY – if none of the cases above takes place.

### Sample input ans output

| eve.in | eve.out |
| ------ | ------- |
|        |         |
| :-----------------------------------------------------: | :-----: |
| ------------------------------------------------------- | ------- |
|                                                         |         |
| 4<br/>M
F
M
F
12
3 4 F
1 2 M
1 2 M
3 4 F
-3
-2
-4
-1
6 5 M
7 5 F
-7
-6
0 |     YES    |
|         3<br/>F
F
M
3
3 2 M
3 1 F
-3
2
4 100500
5 100500         |    YES     |
|                  3<br/>M
F
M
2
1 2 M
3 2 F
0                  |    POSSIBLY     |
|              2<br/>M
F
2
1 2 M
-2
2
1 2011
2 2012              |   NO    |

如果以上表格排版错误，烦请参照下面图片。

![](http://blogphotograph.oss-cn-beijing.aliyuncs.com/18-8-28/65972038.jpg)

## Solution

### Idea

大水题，用并查集即可，把同母的放入一个并查集中。最后进行判断有几个并查集。

(比赛时读错题拐跑了队友，强行增加难度，把新出生的id读成了现在没人用的正数= =)<img src="http://b384.photo.store.qq.com/psb?/V10leItY1b9BLO/s3RTSkVF9jV3iCzY1lsBsZ7SaLU83ze6V3JSEprZek0!/b/dIABAAAAAAAA&bo=ZABkAGQAZAACEDQ!">

### AC Code

```c++
#define MAXN 300010
typedef long long LL;

using namespace std;

int mo[MAXN];
char gendar[MAXN];

map<int, int> dna;
bool life[MAXN];

int n, m, k;
void init()
{
    for(int i=0; i<MAXN; i++)
    {
        mo[i] = i;
    }
}

int ufind(int x)
{
    if(mo[x] == x)
        return x;
    return mo[x] = ufind(mo[x]);
}

void unite(int x, int y)
{
    x = ufind(x);
    y = ufind(y);

    if(x == y)
        return;
    mo[x] = y;
}

bool same(int x, int y)
{
    return ufind(x) == ufind(y);
}

int main()
{
    freopen("eve.in", "r", stdin);
    freopen("eve.out", "w", stdout);
    init();
    scanf("%d", &n);
    getchar();
    for(int i=1; i<=n; i++)
    {
        gendar[i] = getchar();
        life[i] = true;
        getchar();
    }

    scanf("%d", &m);

    int fa, mot, id, shiji;
    char sex;
    int  tot = n;

    for(int i=1; i<=m; i++)
    {
        scanf("%d", &fa);
        if(fa > 0)  //birth
        {
            tot++;

            scanf("%d %c", &mot, &sex);
            id = tot;

            gendar[id] = sex;
            unite(id, mot);
            life[id] = true;
        }
        else    //die
        {
            life[-fa] = false;
        }
    }

    int cnt = tot;
    scanf("%d", &k);
    int seq;
    set<int> qwer;
    
    for(int i=1; i<=k; i++)
    {
        scanf("%d%d", &id, &seq);
        if(dna[seq] != 0)
        {
            // id 和 dna[seq]一个并查集
            unite(id, dna[seq]);
        }
        else
        {
            dna[seq] = ++cnt;
            unite(id, cnt);
        }
    }

    for(int i=1; i<=tot; i++)
        ufind(i);


    set<int> foo;

    for(int i=1; i<=tot; i++)
    {
        if(life[i])
        {
            mot = ufind(i);
            qwer.insert(mot);
            if(mot > tot)
                foo.insert(mot);
        }
    }

    if(qwer.size() <= 1)
        puts("YES");
    else if(foo.size() >= 2)
        puts("NO");
    else
        puts("POSSIBLY");


    return 0;
}
```

