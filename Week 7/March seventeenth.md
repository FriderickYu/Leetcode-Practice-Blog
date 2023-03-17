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

## [Leetcode 322 Coin Change](https://leetcode.com/problems/coin-change/description/)

This question can be abstracted to a complete knapsack problem. `amount` is the capacity j, and `coins[i]` is the item which can be selected for multiple times.

### Determine dp table and index

`dp[j]` represents that in capacity j, we need minimum number of coins

### Determine recursion formula

For coin 2, if capacity is 4, then we know it's `dp[j - coins[i]]`, and don't forget about coin 2, so it's `dp[j - coins[i]] + 1`. `dp[j] = min(dp[j], dp[j - coins[i]] + 1)`

### How to initialize dp table

If capacity is 0, then the number of coins is 0. So `dp[0] = 0`.  Because we wanna find the minimum value, so the rest of `dp[j]` should be `Integer.MAX_VALUE`

### Determine the order of traverse

Because this question is not about permutation or combination, so we don't need to care about nested loop.

### Derive dp by example

<img src="../picture/March%20seventeenth/coins_change.jpg" width = "400" height = "265" alt="coins_change" align=center/>

```java
public int coinChange(int[] coins, int amount) {
    int max = Integer.MAX_VALUE;
    int[] dp = new int[amount + 1];
    dp[0] = 0;
    for(int j = 1; j < dp.length; j ++){
        dp[j] = max;
    }
    for(int j = 1; j <= amount; j ++){
        for(int i = 0; i < coins.length; i ++){
            if(j >= coins[i] && dp[j - coins[i]] != max){
                dp[j] = Math.min(dp[j], dp[j - coins[i]] + 1);
            }
        }
    }
    return dp[amount] == max ? -1 : dp[amount];
}
```
