# Day 41 March seventeenth Dynamic Programming

## [Leetcode 70 Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

This question can be abstracted to a complete knapsack problem, taking `n` steps to reach the top is the capacity j. 1 or 2 steps are the items. Item s can be selected for multiple times.

`dp[j]` represents in j capacity, how many distinct ways we can select

<img src="../picture/March%20seventeenth/climbing_stairs.jpg" width = "400" height = "249" alt="climbing_stairs" align=center/>

```java
public int climbStairs(int n) {
    int[] steps = {1, 2};
    int[] dp = new int[n + 1];
    dp[0] = 1;
    for(int j = 0; j <= n; j ++){
        for(int i = 0; i < steps.length; i ++){
            if(j >= steps[i]){
                dp[j] += dp[j - steps[i]];
            }
        }
    }
    return dp[n];
}
```
