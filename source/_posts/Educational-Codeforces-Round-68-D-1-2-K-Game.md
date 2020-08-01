---
title: Educational Codeforces Round 68 D 1-2-K Game
date: 2019-07-21 19:30:40
tags: [math, 博弈]
---

## Problem

<div align="center">time limit per test : 2 seconds<br>memory limit per test : 256 megabytes<br>input : standard input<br>output : standard output</div>

### Description

Alice and Bob play a game. There is a paper strip which is divided into *n* + 1 cells numbered from left to right starting from 0. There is a chip placed in the *n*-th cell (the last one).

Players take turns, Alice is first. Each player during his or her turn has to move the chip 1, 2 or *k* cells to the left (so, if the chip is currently in the cell *i*, the player can move it into cell *i* - 1, *i* - 2 or *i* - *k*). The chip should not leave the borders of the paper strip: it is impossible, for example, to move it *k* cells to the left if the current cell has number *i* < *k*. The player who can't make a move loses the game.

Who wins if both participants play optimally?

Alice and Bob would like to play several games, so you should determine the winner in each game.

### Input

The first line contains the single integer *T* (1 ≤ *T* ≤ 100) — the number of games. Next *T* lines contain one game per line. All games are independent.

Each of the next *T* lines contains two integers *n* and *k* (0 ≤ *n* ≤ 109, 3 ≤ *k* ≤ 109) — the length of the strip and the constant denoting the third move, respectively.

### Output

For each game, print Alice if Alice wins this game and Bob otherwise.

### Example

#### input

```
4
0 3
3 3
3 4
4 4
```

#### output

```
Bob
Alice
Bob
Alice
```

### Solution

### Idea

打表找规律，存在循环。

发现若 k 不为 3 的倍数，则循环节长度为3 。若 k 为 3 的倍数， 循环节长度为 k+1， 循环节内也有一定的规律。