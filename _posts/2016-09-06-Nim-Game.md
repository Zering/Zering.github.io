---
layout:     post
title:      "尼姆游戏"
subtitle:   " \"LeetCode\""
date:       2016-9-6 13:53:55
author:     "Zering"
header-img: "img/road.jpg"
catalog: true
tags:
    - LeetCode
---
### 问题
题目地址： [https://leetcode.com/problems/nim-game/](https://leetcode.com/problems/nim-game/)

You are playing the following Nim Game with your friend: There is a heap of stones on the table, each time one of you take turns to remove 1 to 3 stones. The one who removes the last stone will be the winner. You will take the first turn to remove the stones.

Both of you are very clever and have optimal strategies for the game. Write a function to determine whether you can win the game given the number of stones in the heap.

For example, if there are 4 stones in the heap, then you will never win the game: no matter 1, 2, or 3 stones you remove, the last stone will always be removed by your friend.

### 问题翻译
你在和你的朋友玩下面这个Nim Game:桌子上面有一堆石头，每次可以拿走一到三颗石头，拿到最后的石头的一方胜利，你将先拿石头。

你们两个都很聪明，都能想到赢得游戏的最佳策略。写一个功能判断你能不能赢得指定数量的石头堆的游戏。

例如，如果有四颗石头，那么你都不可能赢得游戏：不管你拿走1,2，或者3颗石头，最后的石头都会被你的朋友拿走。

### 问题分析
如果在你拿过石头之后能让剩下的石头是4颗，那么你就必胜了；由此，我们轻易能想到，在你拿过石头之后，让石头的数量为4的倍数，之后不管对手拿多少，你都能使石头数变成4的倍数，拿到最后就能必胜；反正，如果一开始石头数量就已经是4的倍数，那你得朋友也一定会让石头数保持4的倍数，就成了必输。

### 问题解答
有分析可知，这是一个非常简单的问题，java代码如下：

	public boolean canWinNim(int n) {
	        return n%4 != 0;
	}