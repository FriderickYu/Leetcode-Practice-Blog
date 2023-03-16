# Day 40 March Sixteenth Dynamic Programming

## Complete Knapsack Problem

Description: There are `n` items, and a knapsack that can carry a maximum weight of `w`. The weight of  $i^{th}$ is `weight[i]`, its value is `value[i]`. Each item **can be used for multiple times, or none**. Solving for which items will be loaded into the knapsack with **the greatest sum of values**.

Taking an expample to illustrate the idea, a knapsack, its maximum capacity is 4


|        | Weight | Value |
| -------- | -------- | ------- |
| Item 0 | 1      | 15    |
| Item 1 | 3      | 20    |
| Item 2 | 4      | 30    |

Almost all steps of complete knapsack are similar with 01 knapsack, except the order of traversal. In the previous 01 knapsack, it's a two nested loop. First is item, then capacity.

The reason why we traverse the capacity from end to start, is that we wanna guarantee each item will be only used for once

<img src="../picture/March%20sixteenth/original_01dp.jpg" width = "400" height = "160" alt="original" align=center/>

```java
for(int i = 0; i < weight.length; i ++){
    for(int j = bagSize; j >= weight[i]; j --){
        dp[j] = Math.max(dp[j], dp[j - weight[i]] + value[i]);
    }
}
```

If we change the order of traversal, from start to end, then it comes to:

<img src="../picture/March%20sixteenth/startToEnd.jpg" width = "400" height = "190" alt="original" align=center/>

```java
for(int i = 0; i < weight.length; i ++){
    for(int j = weight[i]; j <= bagSize; j ++){
        dp[j] = Math.max(dp[j], dp[j - weight[i]] + value[i]);
    }
}
```

```java
private static void CompleteKnapsack(){
    int[] weight = {1, 3, 4};
    int[] value = {15, 20, 30};
    int bagWeight = 4;
    int[] dp = new int[bagWeight + 1];
    for (int i = 0; i < weight.length; i++){
        for (int j = weight[i]; j <= bagWeight; j++){
            dp[j] = Math.max(dp[j], dp[j - weight[i]] + value[i]);
        }
    }
    for (int maxValue : dp){
        System.out.println(maxValue + "   ");
    }
}
```

## [Leetcode 518 Coin Change II](https://leetcode.com/problems/coin-change-ii/description/)

This question can be abstracted to a complete knapsack, because:

1. This is capacity, 4
2. 3 options, and each option can be selected for multiple times

### Determine dp table and index

`dp[j]` represents how many ways we can select coins, in j capacity

### Determine recursion formula

If coin is 2, capacity is 4, then `dp[4] += dp[4 - 2]`

### How to initialize dp table

According to the recursion formula, `dp[0] = 1`

### Determine the order of traverse

This is a complete knapsack problem, so in the second loop, it should be from the start to end

### Derive dp by example:

<img src="../picture/March%20sixteenth/coin_change.jpg" width = "400" height = "505" alt="coin_change" align=center/>

```java
public int change(int amount, int[] coins) {
    int[] dp = new int[amount + 1];
    dp[0] = 1;
    for(int i = 0; i < coins.length; i ++){
        for(int j = coins[i]; j <= amount; j ++){
            dp[j] += dp[j - coins[i]];
        }
    }
    return dp[amount];
}
```

## [Leetcode 377 Combination Sum IV](https://leetcode.com/problems/combination-sum-iv/)

This question can be abstracted to a complete knapsack problem, because:

1. target is the capacity
2. `nums` are the item, which can be selected for multiple times

### Determine dp table and index

`dp[j` represetns how many distinct ways we select numbers, to reach capacity j

### Determine recursion formula

If number is 2, capacity is 4, then `dp[4] += dp[4 - 2]`

`dp[j] = dp[j - nums[i]]`

### How to initialize dp table

According to the recursion formula, `dp[0] = 1`

### Determine the order of traverse

Reminding, this is a permutation problem, not a combination problem. For instance, in permutation problem, `{1, 2}` and `{2, 1}` are two completely different sets. In combination problem, `{1, 2}` and `{2, 1}` are the same.

Because of the complete knapsack problem, in knapsack, it's from start to end. However, in this question, the first loop is knapsack, the second loop is item. **In the combination problem, the first loop is item, but the second loop is knapsack.**

### Derive dp by example:

<img src="../picture/March%20sixteenth/traversal_order.jpg" width = "400" height = "205" alt="traversal_order" align=center/>

```java
public int combinationSum4(int[] nums, int target) {
    int[] dp = new int[target + 1];
    dp[0] = 1;
    for(int j = 0; j <= target; j ++){
        for(int i = 0; i < nums.length; i ++){
            if(j >= nums[i]){
                dp[j] += dp[j - nums[i]];
            }
        }
    }
    return dp[target];
}
```
