---
layout: post
title: 2016中国大学生程序设计竞赛 - 网络选拔赛
subtitle: Gauss, Greedy, Count, computational geometry, string
author: 闫鸿宇
date: 2016-08-14 21:27:32 +0800
categories: [study]
tag: [Gauss, Greedy, Count, computational geometry, string]
comments: true
---

题目[2016中国大学生程序设计竞赛 - 网络选拔赛](http://acm.hdu.edu.cn/search.php?field=problem&key=2016%D6%D0%B9%FA%B4%F3%D1%A7%C9%FA%B3%CC%D0%F2%C9%E8%BC%C6%BE%BA%C8%FC+-+%CD%F8%C2%E7%D1%A1%B0%CE%C8%FC&source=1&searchmode=source)  

代码[在这里](https://github.com/New-bottle/training/tree/master/2016summer/160814)  

# A题
  题意：给一个数n，问是否是73和137的倍数，n的长度为1000W。  
  每次乘10取模就可以了吧？

# B题
  题意：给N个数(n<=300)，其中最大的质因子不超过2000，从中选出一些数，问选出的数的乘积是完全平方数的方案数。  
  做法：发现2000内质数个数大约是300个，选出来的数中包含的所有质数的指数和都必须是偶数，也就是把n个数看做n个01变量，每个质数的指数和都是一个方程，高斯消元解异或方程即可。答案即为 (2^自由元个数)-1

# C题
  题意：有一棵树，每个点有个收益，每条边有个代价，每次经过这条边都要花费代价，但点上的收益只能取一次。问从每个点出发能得到的最大收益分别是多少。  
  树形DP：  

  1. dp[0]表示从x出发只向子树走，最后又回到x的最大收益。
  2. dp[1]表示从x出发向上走，不走x的子树，最后回到x的最大收益（不算x本身的收益）（也即dp[0]+dp[1]就是从x出发绕一圈回来的最大收益）
  3. dp[2]表示从x出发只向子树走，在其中一些子树中兜一圈再向某一子树走下去不回来的最大收益。（枚举向哪个儿子向下延伸，记录最大和次大（初始值都是value[x])  
  4. dp[3]表示从x出发直接向父亲走，不向x的子树走，不回来的最大收益(类似dp[1])


  转移：（这里dp[0][x] - max(0, dp[0][son] - 2 * cost[edge])指从x出发只向子树走，但不走son这个子树，最后又回到x的最大收益）  

  1. dp[0][x] = value[x] + $ \sum $ max(0, dp[0][son] - 2 * cost[edge])  
  2. dp[1][x] = dp[1][fa] - dp[0][fa] - max(0, dp[0][x] - 2 * cost[edge]) - 2 * cost[edge]
  3. dp[2][x] = value[x] + max{max(0, dp[2][son] - cost[edge]) + dp[0][x] - max(0, dp[0][son] - 2 * cost[edge])}
  2. dp[3][x] 有两种情况：  

    - dp[3][fa] + dp[0][fa] - max(0, dp[0][son] - 2 * cost[edge]) - cost[edge] ，即链继续向上延伸  
    - dp[2][fa] + dp[1][fa] - max(0, dp[0][son] - 2 * cost[edge]) - cost[edge] ，即链从fa转向向下延伸。
    这里如果x刚好是fa取最大的dp[2]的那个儿子，则dp[2][fa]需要取次大值。

  ans[x] = max(dp[0][x] + dp[3][x], dp[1][x] + dp[2][x])

# D题
  题意：有10种礼物，每种有a[i]个，你需要给每个人两个礼物，要求相邻两个人的第一个礼物不能相同，但第二个礼物没有要求。问最多能有多少人？  
  贪心，每次从与上一个人选取剩余数量最多的一个
