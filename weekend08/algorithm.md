---
title: 887. 鸡蛋掉落
date: 2019-05-12 18:36:22
categories:  #分类
    - attendance
tags:   #标签
    - ARTS
    - 算法 
description: 
    没有什么，一点思路也没有更没有深想。就当是记录一下吧
---
### 解题思路
难度比较大---困难
[](https://github.com/Shellbye/Shellbye.github.io/issues/42)
```Java
class Solution {

	//
    public int superEggDrop(int K, int N) {
        int[][] dp = new int[K + 1][N + 1];
        for (int m = 1; m <= N; m++) {
            dp[0][m] = 0; // zero egg
            for (int k = 1; k <= K; k++) {
                dp[k][m] = dp[k][m - 1] + dp[k - 1][m - 1] + 1;
                if (dp[k][m] >= N) {
                    return m;
                }
            }
        }
        return N;
    }

    public int superEggDrop(int K, int N) {
        // only one egg situation
        int[] dp = new int[N + 1];
        for (int i = 0; i <= N; ++i)
            dp[i] = i;

        // two and more eggs
        for (int k = 2; k <= K; ++k) {
            int[] dp2 = new int[N + 1];
            int x = 1; // start from floor 1
            for (int n = 1; n <= N; ++n) {
                // start to calculate from bottom
                // Notice max(dp[x-1], dp2[n-x]) > max(dp[x], dp2[n-x-1])
                // is simply max(T1(x-1), T2(x-1)) > max(T1(x), T2(x)).
                while (x < n && Math.max(dp[x - 1], dp2[n - x]) > Math.max(dp[x], dp2[n - x - 1]))
                    x++;

                // The final answer happens at this x.
                dp2[n] = 1 + Math.max(dp[x - 1], dp2[n - x]);
            }

            dp = dp2;
        }

        return dp[N];
    }
}